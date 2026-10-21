---
name: daily-briefing
description: Gives the user a short briefing of today's calendar events and newest unread emails. Use when the user asks for a briefing, a summary of their day, or what is on today.
argument-hint: '{}'
---

# Daily Briefing
Call the `daily-briefing` tool. It needs no arguments. It returns today's events and up to three of the newest unread emails.

## How to reply
- Start with a one-line friendly greeting.
- List today's events in time order as "HH:MM - title". If there are none, say the day is free.
- Say how many unread emails there are, then name the sender and subject of up to three.
- Keep the whole briefing under 80 words, because it is read aloud.
