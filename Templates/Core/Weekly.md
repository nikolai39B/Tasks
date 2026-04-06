---
date:
  "{ date }":
---
# Tasks
```base
filters:
  and:
    - file != this.file
    - or:
        - file.hasLink(link((this.file.properties["date"] + duration("0d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("1d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("2d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("3d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("4d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("5d")).format("MMM DD, YYYY")))
        - file.hasLink(link((this.file.properties["date"] + duration("6d")).format("MMM DD, YYYY")))
    - file.inFolder("Task List")
formulas:
  Location: file.folder.replace("Task List/","")
  Links: |-
    list(file.properties["TAC / Polarion"]).map(

    if(value.toString().startsWith("PR"), link("https://tac.industrysoftware.automation.siemens.com/prs/"+value.toString().replace("PR","")+"/overview", value),

    if(value.toString().startsWith("PES"), link("https://mypolarion.industrysoftware.automation.siemens.com/polarion/#/project/PES2/workitem?id="+value.toString(), value),

    value)))
  Category: file.folder.split("/")[1]
views:
  - type: list
    name: Summary
    groupBy:
      property: formula.Category
      direction: ASC
    order:
      - file.name
      - Description
      - Status
    sort:
      - property: file.path
        direction: ASC
    indentProperties: true

```