# Home Assistant: car wash approval flow

Package: `packages/car_wash_approval.yaml`

## Setup
1. Make sure `configuration.yaml` has
   `homeassistant: { packages: !include_dir_named packages }`, then copy the file into `packages/`.
2. Replace every `CHANGE ME` entity (weather, family calendar, phone notify service) and match the
   `data:` fields of `script.book_car_wash`.
3. Check config, restart, then set the helpers: `Car wash - last wash` and `Car wash - last asked`
   to today's date (they start empty and the flow stays quiet until both are set).

## Tuning (helpers, no YAML edit needed)
- Max days between washes: 30 (monthly minimum)
- Min days between washes: 7
- Dirt threshold: 10 (about 25-50 hours of rain/snow; snow and salty months, Nov-Mar, count more)

## Testing
- Set `Car wash - last wash` 40 days back and `Car wash - last asked` 5 days back, then run the
  `Car wash - propose a booking` automation manually. You should get the notification.
- Developer tools: temporarily make the weather/calendar conditions pass to verify the proposed date.

## Known limits
- The wash is recorded as done on booking. It does not confirm the booking succeeded
  (that depends on what `script.book_car_wash` returns).
- Sundays are skipped. If no good day exists in the next 5 days, it retries the next morning.
- Notifications are in Polish.
