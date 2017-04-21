---
sectionid: stats
sectionclass: h2
title: Stats
number: 25
parent-id: jsonapi-plus
---

Imagine a grid listing records. The [Reads](https://jsonapi-suite.github.io/jsonapi_suite/#reads) section covers populating the grid, but what about "X total records", or "average cost"?

These calculations are supported via `allow_stat`:

```ruby
class EmployeeResource < JsonapiCompliable::Resource
  type :employees
  use_adapter JsonapiCompliable::Adapters::ActiveRecord

  allow_stat total: [:count]
end
```

A GET to `/api/employees?stats[total]=count` would return:

```ruby
{
  data: [...],
  meta: {
    stats: {
      total: {
        count: 100
      }
    }
  }
}
```

A few 'default calculations' are provided: `count`, `sum`, `average`,
`maximum` and `minimum`. These will work out-of-the-box with ActiveRecord.
Alternatively, override these calculation functions:

```ruby
allow_stat :salary do
  average { |scope, attr| scope.average(attr) }
end
```

Or support your own custom calculations:

```ruby
allow_stat salary: [:average] do
  standard_deviation { |scope, attr| ... }
end
```

Multiple stats are supported with one request:

```ruby
GET /api/employees?stats[salary]=average,maximum&stats[total]=count
```

If you want **only** stats, and no records (for performance), simple pass page size 0:

`GET /api/employees?stats[salary]=average&page[size]=0`

{::options parse_block_html="true" /}
<div class='note info'>
###### Further Reading
  <div class='note-content'>
  To learn more about the stats API, see
  [jsonapi_compliable](https://github.com/jsonapi-suite/jsonapi_compliable)
  </div>
</div>
<div style='height: 5rem' />