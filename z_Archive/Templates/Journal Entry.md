- - -
Previous: [[<% tp.date.now("yyyy-MM-DD", -1, tp.file.title) %>]] 
- - -
##### Gratitude

##### Vision

##### Witnessing

### Due Today

```dataviewjs
const shared = dv.pages('"Shared Initiatives"').file.tasks
  .where(t => t.text.includes('{{date:gggg-MM-DD}}'))
  .where(t => t.text.includes('@brandon'))

const personal = dv.pages('-"Shared Initiatives"').file.tasks
  .where(t => t.text.includes('{{date:gggg-MM-DD}}'))

dv.taskList(shared.concat(personal))

```

##### Created Today
```dataview
LIST FROM -"Daily Balance/Journal" WHERE file.cday = date("<% tp.file.title %>")
```

##### Doing - Assigned to Me
```dataviewjs
const shared = dv.pages().file.tasks
  .where(t => t.text.includes('#doing'))
  .where(t => t.text.includes('@brandon'))

dv.taskList(shared)
```

##### Doing - Personal
```dataviewjs
const personal = dv.pages('-"Initiatives - Shared"').file.tasks
  .where(t => t.text.includes('#doing'))

dv.taskList(personal)
```
