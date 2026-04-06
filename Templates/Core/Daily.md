---
date:
  {{date}}
---
# Tasks
```base
filters:
  and:
    - file.hasProperty("status")
formulas:
  folder: file.folder.replace("Task List/","")
  Effort: if(file.properties["Effort (Actual)"].isEmpty(), "--", file.properties["Effort (Actual)"].toString()) + " / " + if(file.properties["Effort (Projected)"].isEmpty(), "--", file.properties["Effort (Projected)"].toString())
  Color: |-
    if(file.properties["Priority"] == 1, "#ffabab", 
    if(file.properties["Priority"] == 2, "#ace7ff", "#aff8db"))
  Links: |-
    list(file.properties["TAC / Polarion"]).map(

    if(value.toString().startsWith("PR"), link("https://tac.industrysoftware.automation.siemens.com/prs/"+value.toString().replace("PR","")+"/overview", value),

    if(value.toString().startsWith("PES"), link("https://mypolarion.industrysoftware.automation.siemens.com/polarion/#/project/PES2/workitem?id="+value.toString(), value),

    value)))
  Resolved: if(file.properties["_resolved"], "Yes", "No")
properties:
  file.name:
    displayName: Task
  note.Effort (Actual):
    displayName: Eff. (A)
  note.Effort (Projected):
    displayName: Eff. (P)
  formula.folder:
    displayName: Location
  note.Priority:
    displayName: P
  note.effort_actual:
    displayName: Eff. (A)
  note.effort_projected:
    displayName: Eff. (P)
  note.priority:
    displayName: P
views:
  - type: cards
    name: Due
    filters:
      or:
        - note["Due Date"] == this.file.properties["Date"]
        - and:
            - note["Due Date"] <= this.file.properties["Date"]
            - formula.Resolved == "No"
    groupBy:
      property: formula.Resolved
      direction: ASC
    order:
      - file.name
      - Description
      - Due Date
      - formula.Effort
      - formula.Links
      - formula.folder
      - tags
    sort:
      - property: Priority
        direction: ASC
      - property: formula.folder
        direction: ASC
    summaries: {}
    cardSize: 300
    image: formula.Color
    imageAspectRatio: 0.07
    columnSize:
      file.name: 268
      note.Status: 100
      formula.Links: 104
      note.Description: 700
      formula.folder: 224
  - type: table
    name: Worked On
    filters:
      and:
        - file.hasLink(this.file)
    groupBy:
      property: formula.Resolved
      direction: ASC
    order:
      - file.name
      - priority
      - status
      - effort_actual
      - effort_projected
      - due_date
      - _external_links
      - description
      - tags
    sort:
      - property: priority
        direction: ASC
    summaries: {}
    cardSize: 300
    image: formula.Color
    imageAspectRatio: 0.07
    columnSize:
      file.name: 268
      note.priority: 59
      note.status: 115
      note.effort_actual: 76
      note.effort_projected: 75
      note.description: 728
    rowHeight: medium

```
