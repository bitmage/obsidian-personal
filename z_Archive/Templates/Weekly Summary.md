---
created: <% tp.date.now("YYYY-MM-DD") %>
updated: <% tp.file.last_modified_date() %>
type: weekly
---
### All Tasks Due This Week

```dataviewjs
dv.taskList(dv.pages().file.tasks
  .where(t => !t.completed)
  .where(t =>
    !![
      '{{monday:gggg-MM-DD}}',
      '{{tuesday:gggg-MM-DD}}',
      '{{wednesday:gggg-MM-DD}}',
      '{{thursday:gggg-MM-DD}}',
      '{{friday:gggg-MM-DD}}',
      '{{saturday:gggg-MM-DD}}',
      '{{sunday:gggg-MM-DD}}'
    ].find(d => t.text.includes(d))
  ))

```

### All Tasks Written This Week

```dataviewjs
dv.taskList(dv.pages('"Daily Balance/Journal"')
  .where(p => 
    !![
      '{{monday:gggg-MM-DD}}',
      '{{tuesday:gggg-MM-DD}}',
      '{{wednesday:gggg-MM-DD}}',
      '{{thursday:gggg-MM-DD}}',
      '{{friday:gggg-MM-DD}}',
      '{{saturday:gggg-MM-DD}}',
      '{{sunday:gggg-MM-DD}}'
    ].find(d => p.file.name.includes(d))
  ).file.tasks

)

```

### Active Initiatives
```dataviewjs
function countComplete(page) {
  return page.file.tasks.where(t => t.completed).length
}

function countAll(page) {
  return page.file.tasks.length
}

function progressBar(page) {
  const percentComplete = (countComplete(page) / countAll(page)) * 100
  return `<progress max=100 value=${percentComplete}> </progress><br/> ${countComplete(page)} of ${countAll(page)}`
}

const activeInitiatives = dv.pages('"Initiatives"')
  .where(p =>
    p.file.frontmatter.type === 'initiative' &&
    p.file.frontmatter.status === 'active'
  )
  .map(p => p.file)

dv.table(['Initiative', 'Progress'], dv.pages('"Initiatives"')
  .groupBy(p => p.file.folder)
  .where(g => activeInitiatives.some(i => i.folder === g.key))
  //.where(g => countAll(g.rows) > 0)
  .map(g => [
    activeInitiatives.find(i => i.folder === g.key).link,
    progressBar(g.rows)
  ]).limit(15),
)
```
### Active Tasks
```dataviewjs
dv.taskList(dv.pages('"Initiatives"').file.tasks
  .where(t => !t.completed)
  .where(t => t.text.includes('#doing'))
)

```

