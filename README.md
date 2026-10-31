# Bazaar
Hypixel Skyblock Bazaar analysing application



Prompt:

# Project: Hypixel SkyBlock Bazaar Market Scanner

Act as a senior software engineer, data engineer and UI/UX designer. Your task is to research, design and build a fully functional application for scanning, filtering, sorting and analysing the entire Hypixel SkyBlock Bazaar.

The goal is to create a comprehensive, general-purpose market analysis tool, not an application hardcoded to a particular arbitrage strategy.

The application must retrieve all available Bazaar items, collect their market metrics, and allow the user to sort, filter, compare and analyse items using freely configurable conditions.

Do not merely create a prototype, mockup or architectural plan. Build the actual working application.

# 1. Technology and implementation freedom

You have complete freedom to choose:

- Programming languages.
- GUI frameworks.
- UI/UX architecture.
- Application architecture.
- Frontend and backend technologies.
- Database technologies.
- Libraries and dependencies.
- Data processing methods.
- Overall code structure.

Do not assume that Python, PySide6 or any other specific technology is required.

Research the available options and select whichever combination you believe will produce the best application.

However, the following requirements are mandatory:

1. The application must have a graphical user interface.
2. The GUI must be visually polished, attractive and easy to operate.
3. The application must be responsive and performant.
4. The architecture must support future expansion.
5. The code must be maintainable and reasonably modular.
6. The finished application must be genuinely functional, not a visual demonstration.

You are responsible for making the architectural and design decisions. Do not ask me to choose frameworks, programming languages, or UI layouts.

Prioritize usability, visual quality, functionality and reliability.

# 2. Data sources and APIs

## Primary source: Hypixel official API

Use Hypixel's official API as the primary source of Bazaar market data.

The most important endpoint is:

`https://api.hypixel.net/v2/skyblock/bazaar`

Investigate the endpoint and its complete response structure.

The endpoint provides information about the entire Bazaar catalogue, including:

- `buy_summary`
- `sell_summary`
- `quick_status`

Investigate and use the following `quick_status` fields:

| Field | Information |
|---|---|
| `buyPrice` | Weighted average buy-side price |
| `sellPrice` | Weighted average sell-side price |
| `buyVolume` | Quantity in active buy orders |
| `sellVolume` | Quantity in active sell orders |
| `buyMovingWeek` | Buy-side transaction volume over seven days |
| `sellMovingWeek` | Sell-side transaction volume over seven days |
| `buyOrders` | Number of active buy orders |
| `sellOrders` | Number of active sell orders |

Investigate the individual entries in `buy_summary` and `sell_summary`, including:

- `pricePerUnit`
- `amount`
- `orders`

Use these entries to retrieve more detailed order information and calculate available order-book metrics.

Do not confuse active order-book quantities with executed transaction volumes.

## Additional Hypixel endpoint

Use the official SkyBlock item metadata endpoint:

`https://api.hypixel.net/v2/resources/skyblock/items`

Use it to enrich the Bazaar data with item information such as:

- Item IDs.
- Display names.
- Categories.
- Rarities.
- Available metadata.

Use Hypixel item IDs as canonical identifiers throughout the application.

## Supplementary sources

The following third-party websites may be used:

- https://sky.coflnet.com
- https://skyblock.finance
- https://www.skyblock.bz

These are supplementary sources, not the foundation of the application.

Use them when:

- Hypixel does not provide a particular metric.
- Historical information is unavailable through Hypixel.
- More detailed trading statistics are available.
- Additional data would materially improve the accuracy of the analysis.

Investigate their APIs and publicly accessible data before implementing scraping.

Prefer structured API access over HTML scraping.

Do not assume that identically named metrics from different sources have identical definitions.

Track the source and timestamp of every externally retrieved metric.

When sources disagree, expose the discrepancies rather than silently combining incompatible values.

## API reliability

Implement:

- Appropriate request intervals.
- Local caching.
- Asynchronous requests where beneficial.
- Sensible concurrency limits.
- Automatic retries with exponential backoff.
- Error handling.
- Graceful handling of unavailable sources.

Respect Hypixel's API policies and rate limits.

Do not require an API key for the public Bazaar endpoint unless official documentation establishes that one is necessary.

# 3. Complete Bazaar scanning

The application must retrieve and process the entire available Bazaar catalogue, not just predefined selections or popular items.

For every item, collect as many of the following metrics as the available sources permit.

### Order information

- Highest buy order price.
- Lowest sell order price.
- Buy order quantity.
- Sell order quantity.
- Total buy order value.
- Total sell order value.
- Number of active buy orders.
- Number of active sell orders.
- Buy/sell price difference.
- Buy/sell price difference as a percentage.
- Order-book depth.
- Individual price-level information, where available.

### Volume and liquidity

- Buy volume per hour.
- Sell volume per hour.
- Buy volume per day.
- Sell volume per day.
- Buy volume per week.
- Sell volume per week.
- Total traded volume.
- Estimated hourly turnover in coins.
- Active buy-side liquidity.
- Active sell-side liquidity.
- Order-book depth at different price levels.

### Historical market information

- Historical buy prices.
- Historical sell prices.
- Historical volumes.
- Price changes over time.
- Percentage price changes.
- Average prices over different periods.
- Historical volatility, if calculable.
- Historical trading activity.

### General information

- Item name.
- Item ID.
- Item category, if available.
- Rarity, if available.
- Data source.
- Last updated timestamp.
- Data freshness.

These are desired metrics, not assumptions about what every source provides.

Only display metrics that can actually be retrieved or calculated reliably.

Where a metric is unavailable, indicate that it is unavailable rather than inventing a value.

# 4. Dynamic filtering system — highest priority

The filtering system must be entirely configurable by the user.

**Never hardcode market criteria into the filtering engine.**

The user must be able to create, modify and remove filtering conditions whenever they want, without changing source code or restarting the application.

Every available numerical metric should be usable as a filtering condition.

### Supported operators

Provide at least the following operators:

- Greater than (`>`)
- Greater than or equal to (`>=`)
- Less than (`<`)
- Less than or equal to (`<=`)
- Equal to (`=`)
- Not equal to (`!=`)
- Between two values.
- Outside a specified range.

Where relevant, also support:

- Contains.
- Does not contain.
- Starts with.
- Ends with.
- Is available.
- Is unavailable.

### Flexible condition builder

Allow users to:

- Add unlimited filtering conditions.
- Remove existing conditions.
- Edit individual conditions.
- Select any available metric.
- Select comparison operators.
- Enter numerical thresholds.
- Define minimum and maximum values.
- Group related conditions.
- Reorder conditions.
- Exclude items matching specified criteria.

Support both AND and OR logical operators.

### Multiple simultaneous filtering conditions

This is essential.

The user must be able to combine as many conditions as necessary.

For example:

`Buy volume > 180 AND Sell volume > 180 AND Buy/sell difference > 2M`

The user must also be able to create complex, nested expressions:

`(Buy volume > 180 AND Sell volume > 180) AND ((Buy order value > 5M AND Buy order value < 15M) OR (Sell order value > 10M AND Sell volume > 500))`

Support arbitrary nesting of condition groups, rather than limiting users to one AND/OR group.

Make the relationship between conditions visually understandable.

The filtering interface should make it easy to construct complex combinations without requiring the user to write code or understand programming syntax.

### Original filter preset

Include the following as the initial example configuration:

- Buy order value: between 5M and 15M.
- Sell order value: between 5M and 15M.
- Buy order value minus sell order value: greater than 2M.
- Buy volume/hour: greater than 180.
- Sell volume/hour: greater than 180.

These are simply default settings.

The user must be able to modify every condition.

Allow users to save, rename, duplicate, export and delete custom filter presets.

# 5. Multi-condition sorting system

Sorting must be as configurable as filtering.

**Do not limit sorting to a single metric or a fixed collection of predefined sorting options.**

The user must be able to apply multiple sorting conditions simultaneously, with each condition having an explicit priority.

For example:

1. Sort by buy/sell difference, descending.
2. If two items have the same difference, sort by buy volume/hour, descending.
3. If those are also equal, sort by sell volume/hour, descending.
4. Finally, sort alphabetically by item name.

Allow users to:

- Add unlimited sorting conditions.
- Remove sorting conditions.
- Choose any available metric.
- Select ascending or descending order independently for each condition.
- Change the priority of sorting conditions.
- Reorder sorting priorities using drag and drop or equivalent controls.
- Combine numerical and alphabetical sorting.
- Apply several sorting conditions simultaneously.

Sorting must work independently of filtering, but both systems must operate together.

For example, the user should be able to filter for items with a buy volume above 500/hour and then sort the remaining items by:

1. Highest percentage spread.
2. Highest sell volume/hour.
3. Highest buy order value.

The application should use stable, predictable sorting with clearly defined tie-breaking behaviour.

Allow custom sorting configurations to be saved alongside filter presets.

### Useful built-in sorting options

Include convenient options such as:

- Highest buy/sell price difference.
- Highest percentage spread.
- Highest hourly buy volume.
- Highest hourly sell volume.
- Highest estimated hourly turnover.
- Largest buy order value.
- Largest sell order value.
- Highest price increase.
- Highest price decrease.
- Highest historical volatility.

These should be shortcuts, not restrictions.

The user must be able to sort by any other available metric as well.

# 6. Results dashboard

Display the complete scanned Bazaar catalogue in a searchable and configurable table.

The table must support:

- Sorting by multiple columns simultaneously.
- Independent ascending and descending directions.
- Multiple simultaneous filters.
- Live updating when filter conditions change.
- Searching for item names and IDs.
- Hiding and showing columns.
- Rearranging columns.
- Adjusting column widths.
- Exporting filtered results to CSV.
- Exporting the complete dataset where appropriate.

Filtering and sorting should operate locally on the collected dataset without requiring new API requests every time a condition changes.

### Dashboard overview

Display:

- Total Bazaar items scanned.
- Number of items matching current filters.
- Last successful scan.
- Current scanning status.
- Data source availability.
- Age of the current market data.
- Number of unavailable or incomplete data fields.

Clearly distinguish between the complete Bazaar catalogue and the currently filtered results.

# 7. Item details and market analysis

Clicking an item should open a detailed view containing all available information.

Include:

- Current buy and sell orders.
- Order quantities and total values.
- Buy/sell spread.
- Hourly, daily and weekly volumes.
- Order-book depth.
- Source-specific metrics.
- Historical prices and volumes.
- Price charts, where historical data permits.
- Data freshness.
- Source discrepancies.

Explain why an item matches the currently active filters.

Also provide a way to inspect the underlying raw data received from each source.

Allow the user to compare items where useful.

# 8. GUI and user experience

The UI/UX design and graphical architecture are entirely your responsibility.

Choose the layout, navigation structure, visual language, component system and interaction patterns that you believe will produce the best user experience.

The only mandatory design requirements are:

- Visually attractive and professionally designed.
- Intuitive and easy to operate.
- Responsive and performant.
- Consistent in its visual language.
- Information-dense without feeling cluttered.
- Suitable for extended periods of market analysis.

Do not blindly follow a predefined layout if you can develop a better one.

Prioritize the usability of the filtering and sorting systems. These are the application's most important interactive features.

In particular, complex filter groups and multiple sorting priorities must remain easy to understand and manage.

Use thoughtful typography, spacing, hierarchy, visual feedback and clear representations of active conditions.

Include helpful defaults, sensible empty states and understandable error messages.

Avoid unnecessary animations and decorative elements that interfere with functionality.

The application should feel like a finished market analysis product, not an ordinary programming project with a basic table attached.

# 9. Data acquisition and architecture

Use a modular architecture with independent data adapters for each source.

Each adapter must:

- Retrieve market data.
- Parse and normalize responses.
- Map item identifiers.
- Convert units where necessary.
- Extract available metrics.
- Track timestamps.
- Report errors.

Keep the following concerns separate:

1. Data acquisition.
2. Data normalization.
3. Data storage.
4. Metric calculation.
5. Filtering.
6. Sorting.
7. GUI presentation.

The filtering and sorting engines must be independent of the GUI framework.

Keep raw source data separate from normalized data so that normalization decisions can be inspected and revised.

Implement:

- Asynchronous requests where appropriate.
- Sensible concurrency limits.
- Local caching.
- Automatic retries with exponential backoff.
- Graceful handling of unavailable sources.
- Background scanning without freezing the GUI.
- Persistent application settings.

A failed source must not prevent the remaining sources from working.

Never fabricate missing values or silently treat unavailable metrics as zero.

# 10. Data interpretation and accuracy

This section is particularly important.

Distinguish between:

- Unit price and total order value.
- Buy order price and sell order price.
- Order quantity and transaction volume.
- Actual hourly volume and calculated average hourly volume.
- Active order-book liquidity and executed transactions.
- Weighted average prices and individual order prices.
- Instant Buy and Instant Sell prices versus buy-side and sell-side order-book prices.

Hypixel provides seven-day moving transaction volumes through `buyMovingWeek` and `sellMovingWeek`.

Where these are used to calculate average hourly volume:

`Hourly volume = Weekly volume / 168`

Make it clear that these are seven-day averages, not actual last-hour transaction counts.

The application may accumulate its own historical snapshots, but must not mistake changes in active order-book quantities for executed transactions.

Calculate order values only when the required price and quantity information is available.

Be especially careful with Hypixel's `buy_summary` and `sell_summary`: these represent returned order-book summaries, not necessarily the entirety of the order book.

Never label the sum of the returned summaries as the total value of the entire book.

Where sources disagree, expose the discrepancies instead of silently combining incompatible metrics.

Allow users to choose which source takes priority for individual metrics where appropriate.

All calculations and metric definitions must be documented.

# 11. Persistence and settings

Save:

- Custom filter configurations.
- Custom sorting configurations.
- Filter presets.
- Sorting priorities.
- Visible columns.
- Preferred data sources.
- Refresh intervals.
- Interface preferences.
- Historical observations, where applicable.

Settings should persist across application restarts.

Allow users to restore default settings.

# 12. Testing

Write automated tests covering:

### Filtering
- Every numerical comparison operator.
- Minimum and maximum boundaries.
- AND/OR logic.
- Arbitrarily nested condition groups.
- Multiple simultaneous conditions.
- Exclusion conditions.
- Missing metric handling.

### Sorting
- Multiple simultaneous sorting conditions.
- Different ascending/descending directions.
- Priority ordering.
- Equal values and tie-breaking.
- Stable sorting.

### Data
- Weekly-to-hourly volume conversion.
- Order-value calculations.
- Source conflicts.
- Item identifier normalization.
- Unavailable APIs.
- Missing data.
- Incorrect or malformed responses.

### Persistence
- Saving and loading filter presets.
- Saving and loading sorting configurations.
- Restoring settings after application restart.

Ensure that changing filtering or sorting conditions immediately updates the results using the existing dataset.

# 13. Development workflow

Follow this sequence:

1. Research Hypixel's official APIs and documentation.
2. Investigate supplementary sources and their APIs.
3. Document available metrics and their definitions.
4. Research and select the optimal technology stack.
5. Design the application architecture and normalized data model.
6. Implement the official Hypixel data adapter.
7. Implement supplementary data adapters where useful.
8. Implement the complete Bazaar scanner.
9. Build the filtering engine.
10. Build the multi-condition sorting engine.
11. Design and implement the GUI.
12. Integrate all components.
13. Test against real Bazaar data.
14. Fix errors and optimize performance.
15. Write installation and usage documentation.

Do not stop after creating an architectural plan or visual prototype.

Continue through actual implementation, testing and refinement.

Make reasonable engineering decisions independently instead of repeatedly asking the user to choose implementation details.

# 14. Deliverables

Provide:

- Complete application source code.
- A working application with a graphical interface.
- Clean and maintainable project structure.
- Dependency requirements.
- Automated tests.
- README with installation and usage instructions.
- Documentation of APIs, data sources and metric definitions.
- Instructions for building and running the application.

The finished application must scan the entire available Hypixel Bazaar catalogue and allow the user to filter, sort, search, compare and analyse items using any combination of available market metrics.

**The central design principle: Build a general-purpose, extensible Bazaar market analysis platform. Data collection should be comprehensive, while filtering, sorting and analysis should be entirely controlled by the user.**
