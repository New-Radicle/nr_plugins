# Google Calendar adapter (calendar)

The calendar capability: create the standing program events and list what is coming up. The calendar is never a state store.

Tool names in Claude Code are `mcp__<server>__<tool>` and the server segment varies per installation. Always match the tool whose name ends in the suffix given below; never hard-code the prefix.

## Gates

| Gate | Rule |
|---|---|
| Data profile | Controlled: this adapter is never offered, even if connected. Regulated: only if Google Calendar is on the allow-list in `company-context.md` > Data-handling profile > Cloud connectors allowed. |
| Approval | `schedule_event` with one or more attendees sends invitations, so it is gated: show the title, time, every attendee, and the notes, stop, and proceed only when the Sponsor or Rollout Lead says yes in this chat. One approval covers one event. An event with no attendees on the user's own calendar is not gated. |
| Persona data | Persona names never appear in a title, description, or attendee list. `personas.private.md` stays in the local folder. |

## Setup

1. Match `list_calendars`. Ask the user which calendar the program uses; default to their primary calendar.
2. Record the calendar id and the user's time zone in `company-context.md` > Capabilities > Calendar notes. Skills read them from there.
3. Every program event title starts with `AI adoption:` so `search_events` can find it later.

## Operations

| Operation | Tool suffix | Parameters |
|---|---|---|
| `schedule_event(title, when, attendees, notes)` | `create_event` | calendar id from company-context; summary = `AI adoption: <title>`; start and end in the recorded time zone; attendees = the list as approved; description = notes plus the week number; before creating, run the idempotency search below |
| `list_upcoming(days)` | `list_events` | calendar id, from today to today plus `days`; filter to titles starting with `AI adoption:`; report title, date, and attendee count |
| Find an existing program event | `search_events` | query on the title; used before every create and by `week` to show what is scheduled |
| Propose a slot | `suggest_time` | attendees and duration; show the options; the user picks; no event is created by this step |
| Move or rename an event | `update_event` | event id from the search; only the fields the user named |
| Cancel an event | `delete_event` | only when the user asks in this chat; say who will be notified |
| Read one event | `get_event` | event id; used to confirm details before an update |
| `respond_to_event` | not used by skills | the user answers invitations themselves |

## Standing program events

Dates are computed from `start_date` (the Monday of Week 0) in `companies`. Durations are defaults the user can change.

| Week | Title | Default | Attendees | Gate |
|---|---|---|---|---|
| 0 | Sponsor alignment | 60 min | Sponsor, Rollout Lead | yes |
| 1 | Kickoff | 30 min | whole team or department | yes |
| 1 to 2 | Hands-on workshop | 120 min | whole team or department | yes |
| 1 to 12 | Weekly check-in reminder | 30 min, same weekday each week | Rollout Lead only | no |
| 7 | Office hours | 60 min | open; team invited | yes |
| 8 | Mid-point survey reminder | 15 min | Rollout Lead only | no |
| 11 | Impact report presentation | 45 min | Sponsor, Rollout Lead, team leads | yes |

Founder mode creates only the weekly check-in reminder and the Week 6 impact note reminder, both with no attendees.

The plugin cannot make an event recurring. For the weekly check-in it creates the first one and says so; the user either sets the recurrence in Calendar by hand or creates a Routine (scheduled task) that runs `weekly-checkin`. Do not create twelve separate events unless the user asks.

## Keys and idempotency

The key of a program event is its title plus its week. Before every `create_event`, `search_events` on the title within the program window; if a match exists, offer `update_event` instead of creating a duplicate. Record each created event id in `company-context.md` > Notes and decisions with the week and date so a later session can find it without searching.

## Generated documents

The calendar holds no documents. Agendas or pre-reads referenced in a description live in `outputs/` or the state store and end with:

`AI-assisted draft. Reviewed by: ________ (name, date)`

The adapter never fills the blank. Event descriptions carry no review line.

## Privacy

Attendee lists come from the user, never from a directory. Descriptions hold the agenda and the week, not scorecard numbers per person, not blockers attributed to individuals. `list_events` results are used for the program view only; other events on the calendar are not read into any table.

## Failure paths

| Condition | What to do |
|---|---|
| No tool ending in `create_event` in this session | Say the calendar adapter is not connected this session; print the event under "Add this to your calendar"; do not switch the adapter in company-context |
| Error mentions unauthorized, expired, or token | Say the Google authorization has expired and needs reconnecting; use the print fallback for this run |
| Calendar not found or no write access | Say which calendar id was refused; offer the primary calendar or the print fallback |
| `create_event` errors after approval | Report it; do not recreate without a fresh yes; never retry silently, because a silent retry can send duplicate invitations |
| Time zone missing | Ask once, record it in company-context, then continue |

Microsoft 365 is not covered: its connector is search-only in this release, so Outlook users use the print fallback for events and the files adapter for state.
