---
name: "Library/baudogit/TaskCommander"
tags: meta/library
files:
- TCreference.md
- TCreference.pdf
- TCexport.md
- TCstyles.css
- TCicons.svg
- where-examples_initial.txt
- where-history_initial.txt
pageDecoration.prefix: "📋 "
maj: 2026-03-02
---
  
# 📋Task Commander

A toolbox for your tasks🧰:

✨Display a list of tasks in a SilverBullet panel (**css view** or **raw view**)
✨**Extract** the tasks from the index with custom queries, editing function and input assistance
✨**Filter** and **sort** the list with many criteria. **Export** and **print** the list to html or md format
✨**Update status**. **Navigate** to the original page in the main window or in a floating window
✨**Manipulate attributes** (add, delete, rename, calculate and edit statistics) with **batch processing**
✨Use a **command line** with intuitive syntax

![TaskCommander|800px](https://raw.githubusercontent.com/Baudogit/silverbullet-libraries/refs/heads/main/screenshots/TaskCommander.png)
To start, use ${widgets.button("System: Reload", function() editor.invokeCommand("System: Reload") end)} then ${widgets.button("Toggle Task Commander", function() editor.invokeCommand("Navigate: Toggle Task Commander") end)} or `Ctrl+Alt+v` or “list” button added to your SilverBullet toolbar by this space-lua:

````space-lua
-- If you don't want the button in your toolbar, rename this space-lua in lua
actionButton.define {
  icon = "list",
  description = "Task Commander Alt+Ctrl+v",
  priority = 1,
  run = function()
    editor.invokeCommand("Navigate: Toggle Task Commander")
  end
}
````


> **warning** Opening Task Commander
>  Depending on how the panel was closed by Task Commander or by another plugin, it may be necessary to click the command button twice. To avoid this, **close** Task Commander with toggle command, toggle button or the X of Task Commander (but not the “⯌“ of multi-windowing system).

---

# Features

## List of tasks

When opening the widget, the panel you have chosen displays all your tasks or just those that match the ==custom where clause== defined in your Configuration file (see below).

- **Each task** is displayed on multiple lines:
    - First lines (one or more): the ==text== of the task, without attributes. Split text, broken down between attributes, is concatenated with "|" separator.
    - Following lines (as many as necessary): all the ==attributes==
    - Last line (when break page is not selected): ref (==name_of_page_with_path@position==)
    - Tooltip: ==text in raw mode== (without css rules) + ==reference== + ==itags==
- Lines are formatted with your **css rules** (those applied in markdown pages) or with **highligh structural** elements depending on the option selected with toggle button `[ ]` on the toolbar.
- The list can be personalized with the **toolbar**:
    - ==Page break==: toggle page break
    - ==Completed tasks==: toggle if completed tasks are included or not
    - ==Reset==: extract all the tasks or reexecute the default query (see below Configuration)
- A **status bar** permanently indicates how the list was generated
- **Navigate** to the original task page clicking on a line, according to the option chosen with the toggle button on the toolbar:
    - button is red: in the main editor window
    - button is green: in a separate window (if multi-windowing system is installed)
- You can **change** the ==status== of a task in the panel or on its original page
  
> **warning** Custom statuses are displayed but cannot be updated via Task Commander
>   To do this: navigate to the original page and update the task manually

## Command line

Actions are triggered via a ==**command line**==. To access it, click in the input box.
The text entered in the box is used:
- to **filter** the list with ==any character, special groups, **itags** and operators==
- or to **sort** the list according to the ==**value** of one or more attributes==
- or to **calc** (+|-) the value of a new attribute according to values of other attributes of the task
- or to **update** attributes of tasks with ==**batch processing**== (add, delete, rename)
- or to edit **statistic** (amount of the value of one or more attributes) of the tasks in the list
- or to **query** the index with a ==**custom Where clause**==

> **danger** **Update with batch processing**
>   This feature is experimental. It is recommended to use it on a copy of your document space.

> **note** Syntax   **!!! Please read detailled rules by clicking `💡` on the command line !!!**
>   **To filter**: 
>   WHAT: characters OR groups. Groups are delimited by double or single quotes (strings), double brackets (attribute with quotes neutralized) or curly brackets (tags prefixed with # in text or itags).
>   CHAINED by operators “+“ or “-” or “ “ (space = “OR”)
>   **To sort**: one or more attribute’s name followed by "+" or "-" (asc/desc). Type of values recognized: string, number, date (many formats). Specific for Sort: `!` to keep all the lines in the list.
>   **To calc** : idem than Sort, then “`->`“ to define the name of the new attribute
>   **To update**: `verb` (add, delete, replace) + “`:`“ + `name of attribute` (value can be provided when adding) and “`->`“ when replacing. Spaces are not meaningful.
>   **To edit stats**: one or more attribute’s name
>   **To query**: write your custom where clause (without “where”) with extended LIQ syntax

> **success** **Popup assistance**
>  Type one ou two `/` in the inpup box to display a popup:
>       **`/`**: names (or values according to the position of **`/`**) of **attributes** collected in the task list
>      **`//`** (Filter or Query mode): **itags** collected in the task list (without the tag “task”)
> To select an item and insert it in the input, use the keyboard (alphabetical search or up/down arrows) or the mouse, then Enter to validate or ESC to exit.
> You can choose an ==operator adapted== to the current action in the popup.
> Item (and operator if one selected) are inserted into the input with a ==format adapted== to the action.

> **success** **Format assistance (Filter mode only)**
>      **`///`**: insert `[[]]` on the command line to define a ref (name_of_page_with_path@position)

> **success** **Menu assistance (Query mode only)**
>  In addition to popups, three submenus appear in which you can select an item:
>      + standard **properties** of a task
>      + **examples** of custom where clauses
>      + **history** of executed clauses (limited to the number defined in Configuration or 25 by default)

The **input line** accepts direct keyboard entry, copy-paste operation and paste from the popup or the menu. To hide the popup, press **Esc** or **left click** anywhere (in or out of the input, but not in the menus). By pressing **Esc again**, the input box is cleared.
To show the button line (and the menus) again **right click** on the input.

    Notes about the workflow:
    - Filter is not preserved when you sort the list but you can filter after a sort.
    - Filter is preserved when you update the attributes. So, you can extract, filter and then, update.
    - To remove a filter or a sort, or to clear the command line, click on the `X` button at the right.
    - To remove a sort, you can also click the button "Toggle break page" on the toolbar.

> **note** Trigger action
>      Filter mode: immediate application (live) - nothing to do
>      Query, sort, calc, update and stats mode : press **Enter** to trigger the action
  
---

# Environment

## Multi-windowing

These functionalities are developed and diffused [by Mr.Red](https://community.silverbullet.md/t/advanced-panel-controls-e-g-resizing-side-panels-lhs-rhs-bhs-using-your-mouse/3728/33)
via his library `UnifiedAdvancedPanelControl.js`.

The concept uses two types of iframe:
- the three SilverBullet panels with **resizing and moving capabilities**. Theses panels are designated by their position: left= ==lhs== | right= ==rhs== | bottom= ==bhs==).
- **synthetic** panels, in unlimited numbers, designed on the fly.

> **warning** Limit
>   The `bhs` panel is only partially supported by Mr.Red's library

**Default panel for Task Commander is ==rhs==**. You can modify it with `config.set` command, like this :
```lua
-- change lua block to space-lua or copy this line in your config.md
config.set("explorer2", { position = "bhs" })
```

> **note** lhs, rhs, bhs
>  Each panel resizable is unique. When multiple tools use the same panel, the last activated replaces the previous one in the panel. This generally poses no problem but requires double calling to open it with toogle command (via keyboard shortcut or button).

## Styling

> **warning** Limit
>      Currently, Task Commander is not entirely compatible with **dark mode**

1) **Styles for Task Commander**: they are essentially managed by `TCstyles.css`.
   The section: `11. MAIN UI (Colors)` can be customized in a `space-style` within your space.

A colored gauge is used by Task Commander when a query is executed or batch processing:
```space-style
/* You can modify it here or by copying this line into your config style and then suppr this one */
html[data-theme="light"] {
  --progress-mon-type-color: #00E439;   /* light green */
}
```
  
2) **Styles for tasks**: the rules already defined in your space-styles are used by Task Commander. No need to change anything in your configuration.

These styles can be finely customized since SilverBullet’s version 2.40.

> **warning** Edge version - New span classes for attributes
>  The classes used for the name and the value of an attribute are modified as of the edge version published 2026-02-04. Since this version:
>     `sb-attribute-name` is used for the attribute name. This classe replaces `sb-atom`.
>     `sb-attribute-value` is used for the attribute value. Previously, this span had no class.
>  Task Commander detect the version in execution since edge version of 2026-02-06 (not 04).

Rules defined in the `space-style` below applies to `attr-alt-display` class injected with the button “[ ]“ on the toolbar. It ==reveals the structural elements, the name and the value== of the attributes:

```space-style
/* priority: 10 */

/* for attr-alt-display class after it has been injected into the iframe body with js*/
body.attr-alt-display .sb-attribute {
  background: yellow;
  color: black;
  border: 1px solid orange !important ;
}
body.attr-alt-display .sb-attribute> .sb-list.sb-frontmatter.sb-meta {
    display: inline !important;
}
body.attr-alt-display .sb-attribute> .sb-list.sb-frontmatter.sb-attribute-name {
    display: inline !important;
}
body.attr-alt-display .sb-attribute> .sb-list.sb-frontmatter.sb-attribute-value {
    display: inline !important;
}
```

---

# Implementation

## Installation

1) Install **Task Commander**. Clic the command ${widgets.commandButton "Library: Install"} and use this URI:
https://github.com/Baudogit/silverbullet-libraries/blob/main/TaskCommander.md
All the necessary files are copied in the folder: `Library/baudogit/`.

> **note** Note
>  During installation, two text files for custom Where clauses are copied: one offers examples ; the other records a history. A client version of these files is created when Task Commander is installed and system reloaded. These client versions are preserved during subsequent updates.

2) Install **Document Explorer** by Mr.Red with Library Manager, via his repository:
      https://github.com/Mr-xRed/silverbullet-libraries/blob/main/Repository/Mr-xRed.md
    `UnifiedAdvancedPanelControl.js` is copied in the folder `Library/Mr-xRed/`.

   Note: You can use Task Commander without `UnifiedAdvancedPanelControl.js` but, in this case, you will not have access to multi-window functionalities.
   
## Configuration

Copy the code below into your `space-lua` configuration (all items are **optional**):
````lua
config.set("explorer2", {
  -- panel to use
  position = "rhs",
  -- query executed when opening Task Commander
  where = "not done and some(_.attrib1) ~= nil and string.sub(tostring(_.attrib1,1,10)) >= '2025-01-12'",
  -- if this request must be re-executed on each reset
  whereAlways = "true",
  -- maximum number of lines in history
  maxWhereHistory = 25
})
````

**Discussions (community board)**:

- Task Commander: 
- Document Manager https://community.silverbullet.md/t/document-explorer-for-silverbullet/3647/159

See also :
- Task Manager by Mr.Red: https://community.silverbullet.md/t/todo-task-manager-global-interactive-table-sorter-filtering/3767
- Kanban integration by Mr.Red: https://community.silverbullet.md/t/kanban-integration-with-tasks/925/20

**Upcoming developments**:
- Interface: adapt style to dark theme
- Query mode: extend list of attributes (all in the index rather than in the list)

---

::: ==space-lua 1== (main) ::: CONFIG | REFRESH | UPDATE | HISTORY FILE | **DRAW PANEL** | COMMANDS

    Details for DRAW PANEL:
TOOLBAR | BREADCRUMB | STATUS BAR | **HTML TASKS LIST** | **JS SCRIPT** | SHOW PANEL |

    Details for JS SCRIPT:
STYLES | PRINT | UPDATE TASK STATUS | FILTERING | SORT | CALC | POPUP | SUBMENUS | MENU MODES | EVENT LISTENERS | EXECUTE MODE | INITIALISATION | OPENING TASK |

    Details for HTML TASKS LIST:
::: ==space-lua 2== ::: QUERY | BUILD HTML | TEMPLATES
::: ==space-lua 3== ::: FORMAT PAGE | FORMAT TASK

---

```space-lua
-- priority: -1
-- ::: space-lua 1 :::

-- ----------CONFIG ----------

config.define("explorer2", {
  type = "object",
  properties = {
    position = schema.string(),
    where = schema.string(),
    whereAlways = schema.string(),
    maxWhereHistory = schema.number()
  }
})

-- Icons
local ICONS = {
  list       =
  "<svg class='icon-list taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-list'></use></svg>",
  tree       =
  "<svg class='icon-tree taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-tree'></use></svg>",
  refresh    =
  "<svg class='icon-refresh taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-refresh'></use></svg>",
  close      =
  "<svg class='icon-close taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-close'></use></svg>",
  squareplus =
  "<svg class='icon-square-plus taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-square-plus'></use></svg>",
  square     =
  "<svg class='icon-square taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-square'></use></svg>",
  brackets   =
  "<svg class='icon-brackets taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-brackets'></use></svg>",
  fileup     =
  "<svg class='icon-file-up taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-file-up'></use></svg>"
}

-- Load config
local cfg = config.get("explorer2") or {}
local PANEL_ID = cfg.position or "rhs"
local WHERE_INIT = cfg.where or ""
local WHERE_ALWAYS = cfg.whereAlways or "false"
local maxEntries = cfg.maxWhereHistory or 25

-- SilverBullet's version
local version = system.getVersion()
local date = string.match(version, "(%d%d%d%d%-%d%d%-%d%d)")
if date == nil then date = "2000-01-01" end
js.window.sessionStorage.setItem("dateVersion", date)

-- Others variables and constants
local PANEL_VISIBLE = false
local VIEW2_MODE_KEY = "modeView"

js.window.sessionStorage.setItem("searchMode", "list")
js.window.sessionStorage.setItem("searchInit", "true")
js.window.sessionStorage.setItem("searchTerm", WHERE_INIT)
js.window.sessionStorage.setItem("searchTermFilter", "")

-- ---------- INITIALIZE CUSTOM CLAUSE WHERE FILES ----------

local function initializeConfigFile(fileName, initialFileName)
  local filePath = "Library/baudogit/" .. fileName
  local initialPath = "Library/baudogit/" .. initialFileName

  local exists = pcall(function()
    space.readFile(filePath)
  end)

  if not exists then
    local success, content = pcall(function()
      return space.readFile(initialPath)
    end)

    if success and content then
      pcall(function()
        space.writeFile(filePath, content)
      end)
    end
  end
end

initializeConfigFile("where-examples.txt", "where-examples_initial.txt")
initializeConfigFile("where-history.txt", "where-history_initial.txt")

-- ---------- TO UPDATE ATTRIBUTE ----------
-- Pending resolvers and global single listener
pendingModals = {}
event.listen {
  name = "updateConfirmResultTC",
  run = function(e)
    local id = e.data.id
    local value = e.data.value
    local resolve = pendingModals[id]
    if resolve then
      resolve(value)
      pendingModals[id] = nil
    end
  end
}

-- ---------- UPDATE LIST----------

-- Reset tasks list
function refreshExplorer2Button()
  if WHERE_ALWAYS == "true" then
    js.window.sessionStorage.setItem("searchTerm", WHERE_INIT)
    drawPanel("maj", WHERE_INIT)
  else
    js.window.sessionStorage.setItem("searchTerm", "")
    drawPanel("maj")
  end
end

-- Toggle with or without completed tasks
function toggleQueryButton()
  local whereClause = ""
  local current = clientStore.get("explorer2.queryAll")
  if current == "true" then
    clientStore.set("explorer2.queryAll", "false")
  else
    clientStore.set("explorer2.queryAll", "true")
  end

  -- Preserve custom query, if one exists
  local savedSearchTerm = js.window.sessionStorage.getItem("searchTerm");
  if savedSearchTerm ~= nil and savedSearchTerm ~= "" then
    whereClause = savedSearchTerm
  end

  drawPanel("maj", whereClause)
end

-- ---------- UPDATE TASK ----------

-- 1) TOGGLE STATUS
local function toggleTaskRemote(pageName, pos, currentState, statePerso)
  local content = space.readPage(pageName)
  if not content then return end

  if statePerso ~= "" then
    editor.flashNotification("Custom status. No modification!")
    return
  end

  editor.showProgress(0, "custom")

  -- Edit the original line (string between brackets at the start)
  local lineEnd = content:find("\n", pos + 1) or (#content + 1)
  local originalLine = content:sub(pos + 1, lineEnd - 1)

  local newLine = ""
  local timestamp = os.date("%Y-%m-%d %H:%M")

  if currentState == " " or currentState == "" then
    local cleaned = originalLine:gsub("%[%s*%]", "[x]")
    cleaned = cleaned:gsub("%s*%[completed: [^%]]+]", "")
    newLine = cleaned .. " [completed: " .. timestamp .. "]"
  else
    local cleaned = originalLine:gsub("%[[xX]%]", "[ ]")
    newLine = cleaned:gsub("%s*%[completed: [^%]]+]", "")
  end

  local prefix = content:sub(1, pos)
  local suffix = content:sub(lineEnd)
  local finalContent = prefix .. newLine .. suffix
  space.writePage(pageName, finalContent)

  js.window.setTimeout(function()
    -- Preserve custom query, if one exists
    local whereClause = ""
    local savedSearchTerm = js.window.sessionStorage.getItem("searchTerm");
    if savedSearchTerm ~= nil and savedSearchTerm ~= "" then
      whereClause = savedSearchTerm
    end
    drawPanel("maj", whereClause)
    editor.showProgress()
    editor.flashNotification("1 task updated")
  end, 100) -- an awaitEmptyQueue will be applied just before querying the tasks
end

-- 2) UPDATE ATTRIBUTE

-- 2.1) Modify task line
local function modifyTaskLine(content, pos, action, attrName, newValue)
  local lineEnd = content:find("\n", pos + 1) or (#content + 1)
  local originalLine = content:sub(pos + 1, lineEnd - 1)
  local newLine = originalLine
  local modified = false

  if action == "add" then
    local pattern = "%[" .. attrName .. ":%s*[^%]]*%]"
    if originalLine:match(pattern) then
      modified = false
    else
      local value = newValue
      if value == "" then
        value = ".."
      end
      newLine = originalLine .. " [" .. attrName .. ": " .. value .. "]"
      modified = true
    end
  elseif action == "delete" then
    newLine = originalLine:gsub("%s*%[" .. attrName .. ":%s*[^%]]+%]", "")
    modified = (newLine ~= originalLine)
  elseif action == "rename" then
    newLine = originalLine:gsub("%[" .. attrName .. ":", "[" .. newValue .. ":")
    modified = (newLine ~= originalLine)
  end

  if not modified then
    return nil, false
  end

  local prefix = content:sub(1, pos)
  local suffix = content:sub(lineEnd)
  return prefix .. newLine .. suffix, true
end

-- 2.2) Batch processing
local function updateTaskAttributes(taskIds, action, attributeName, newValue)
  editor.showProgress(0, "Analyzing...")

  -- Group by page
  local pageGroups = {}
  for _, taskId in ipairs(taskIds) do
    local pageName, posStr = taskId:match("(.+)@(%d+)")
    if pageName and posStr then
      local pos = tonumber(posStr)
      if not pageGroups[pageName] then
        pageGroups[pageName] = {}
      end
      table.insert(pageGroups[pageName], pos)
    end
  end

  -- BLANK LOOP to count the modifications to be made
  local modifCount = 0
  local modifPerPage = 0
  local logPos = ""
  local logAction = ""
  local logPages = ""

  -- LOG
  logAction = "************ TO UPDATE *************\n"
  logAction = logAction .. "Action: " .. action .. " | Attribute: " .. attributeName .. " | String: " .. newValue
  js.log(logAction)

  for pageName, positions in pairs(pageGroups) do
    table.sort(positions, function(a, b) return a > b end)
    modifPerPage = 0
    logPos = "\nTasks position: "

    for _, pos in ipairs(positions) do
      local content = space.readPage(pageName)
      if content then
        local _, wasModified = modifyTaskLine(content, pos, action, attributeName, newValue)
        if wasModified then
          modifCount = modifCount + 1
          modifPerPage = modifPerPage + 1
          logPos = logPos .. pos .. "-"
        end
      end
    end

    -- Feedback
    if modifPerPage == 0 then
      --js.log("Page: " .. pageName .. " :: no task to update")
      logPages = logPages .. "\nPage: " .. pageName .. " :: no task to update"
    else
      --js.log("Page: " .. pageName .. " :: " .. modifPerPage .. " tasks to update" .. logPos)
      logPages = logPages .. "\nPage: " .. pageName .. " :: " .. modifPerPage .. " tasks to update" .. logPos
    end
  end
  if modifCount == 0 then
    editor.flashNotification("No tasks to update")
    editor.showProgress()
    return
  end

  -- REQUEST FOR CONFIRMATION
  local actionText = action == "add" and "Adding" or (action == "delete" and "Deleting" or "Renaming")
  local attrText = action == "rename" and (attributeName .. " -> " .. newValue) or attributeName
  local confirmMessage = actionText .. " attribute [" .. attrText .. "] on " .. modifCount .. " task(s). Continue?"
  local displayLog = logAction .. logPages

  local callId = tostring(os.time() .. math.random())
  local result = nil

  -- Registers the resolver for this call
  pendingModals[callId] = function(value)
    if value ~= "yes" then
      editor.flashNotification("Update cancelled !!")
      editor.showProgress()
      return
    end

    -- REAL TREATMENT
    editor.showProgress(0, "Updating attributes...")

    for pageName, positions in pairs(pageGroups) do
      -- Sort by position descending to avoid offset shifts
      table.sort(positions, function(a, b) return a > b end)

      local content = space.readPage(pageName)
      if content then
        local workingContent = content
        for _, pos in ipairs(positions) do
          local newContent, wasModified = modifyTaskLine(workingContent, pos, action, attributeName, newValue)
          if wasModified then
            workingContent = newContent -- Accumulate
          end
        end
        -- Write ONLY ONCE after all changes to the page
        if workingContent ~= content then
          space.writePage(pageName, workingContent)
        end
      end
    end

    js.window.setTimeout(function()
      -- Preserve custom query, if one exists
      local whereClause = ""
      local savedSearchTerm = js.window.sessionStorage.getItem("searchTerm");
      if savedSearchTerm ~= nil and savedSearchTerm ~= "" then
        whereClause = savedSearchTerm
      end
      drawPanel("maj", whereClause)
      editor.showProgress()
      editor.flashNotification(modifCount .. " task(s) updated")
    end, 100) -- an awaitEmptyQueue will be applied just before querying the tasks
  end

  js.window.showUpdateConfirmationModal(displayLog, confirmMessage, callId)
end

-- 2.3) Batch update for Calc mode: receives parallel arrays (taskIds, attrName, values)
-- No confirmation (JS side handles it)
local function calcBatchUpdate(taskIds, attrName, values)
  if not taskIds or #taskIds == 0 then return 0 end

  -- Group by page, each entry carries its own value
  local pageGroups = {}
  for i, taskId in ipairs(taskIds) do
    local pageName, posStr = taskId:match("(.+)@(%d+)")
    if pageName and posStr then
      local pos = tonumber(posStr)
      if not pageGroups[pageName] then
        pageGroups[pageName] = {}
      end
      table.insert(pageGroups[pageName], { pos = pos, value = values[i] })
    end
  end

  -- Process page by page
  local totalModified = 0
  for pageName, items in pairs(pageGroups) do
    -- Sort by position descending to avoid offset shifts
    table.sort(items, function(a, b) return a.pos > b.pos end)

    local content = space.readPage(pageName)
    if content then
      local workingContent = content
      for _, item in ipairs(items) do
        local newContent, wasModified = modifyTaskLine(workingContent, item.pos, "add", attrName, item.value)
        if wasModified then
          workingContent = newContent
          totalModified = totalModified + 1
        end
      end
      if workingContent ~= content then
        space.writePage(pageName, workingContent)
      end
    end
  end

  return totalModified
end

-- ---------- HISTORY FILE OF WHERE CLAUSES ----------

-- Write query
local function writeToHistory(whereClause)
  if not whereClause or whereClause == "" then return end
  local historyFile = "Library/baudogit/where-history.txt"
  local contentBinary = space.readFile(historyFile) or ""
  local content = encoding.utf8Decode(contentBinary);

  local lines = {}

  -- Format: "xx | whereClause"
  for line in content:gmatch("[^\r\n]+") do
    local cleaned = string.gsub(string.gsub(string.gsub(line, '^"', ''), '",$', ''), '"$', '')
    if cleaned ~= "" then
      table.insert(lines, cleaned)
    end
  end

  -- Avoid duplicates
  local clauseOnly = whereClause:match("^%d+ | (.+)$") or whereClause
  for i, line in ipairs(lines) do
    if line:match("^%d+ | (.+)$") == clauseOnly then
      table.remove(lines, i)
      break
    end
  end

  table.insert(lines, 1, clauseOnly)
  while #lines > maxEntries do
    table.remove(lines)
  end

  local newContent = ""
  for i, line in ipairs(lines) do
    local num = string.format("%02d", maxEntries - i + 1)
    local clause = line:match("^%d+ | (.+)$") or line
    newContent = newContent .. '"' .. num .. " | " .. clause .. '",\n'
  end

  local newContentBinary = encoding.utf8Encode(newContent)
  space.writeFile(historyFile, newContentBinary)
end

-- ---------- DRAW PANEL ----------
local function drawPanel(seeMess, clauseWhere)
  local currentWidth = clientStore.get("explorer2.panelWidth") or config.get("explorer2.panelWidth") or 0.8
  local viewMode = clientStore.get(VIEW2_MODE_KEY) or "list"
  local filterEnabled = clientStore.get("explorer2.disableFilter") ~= "true"
  local allTasks = clientStore.get("explorer2.queryAll") == "true"

  editor.showProgress(20, "DRAW...")
  local h = {}

  table.insert(h, [[<div data-panel="myPanel" class="explorer-panel mode-]])
  table.insert(h, viewMode)

  -- ---------- HEADER -----------
  js.window.sessionStorage.setItem("searchTermFilter", "")
  local searchTermCh = js.window.sessionStorage.getItem("searchTerm")
  table.insert(h, [[">
            <div class="explorer-header">
              <div class="explorer-toolbar">
                <div class="input-wrapper">
                  <input id="tileSearch" title="Command bar" placeholder="&nbsp;&nbsp;&nbsp;&nbsp;...&nbsp;&nbsp;Please choose Filter, Query or Update" </input>
                  <span id="inputClearBtn" title="Clear input" onclick="clearSearchInput()">&times;</span>
                </div>
                    ]])
  local filterDisabled = clientStore.get("explorer2.disableFilter") == "true"
  local activeClass = filterDisabled and " active" or ""
  table.insert(h, [[
                <div class="view-switcher">
                  <div title="Toggle: page opening mode"
                          class="explorer-action-btn]] .. activeClass .. [[" id="openingMode-btn"
                          onclick="syscall('editor.invokeCommand','TaskExplorer: Toggle Opening Mode')">]])
  table.insert(h, (filterDisabled and "🟢" or "🟠"))
  table.insert(h, [[
                  </div>
                  <div title="Toggle: show/hide completed"
                        class="]])
  table.insert(h, (allTasks and "active" or ""))
  table.insert(h, [[" id="tree-toggle-btn"
                        style="display: ]])
  table.insert(h, (viewMode == "list"))
  table.insert(h, [[" onclick="syscall('lua.evalExpression', 'toggleQueryButton()')">
                        <span id="tree-toggle-icon">]])
  if allTasks then
    table.insert(h, ICONS.squareplus)
  else
    table.insert(h, ICONS.square)
  end
  table.insert(h, [[</span></div>
                  <div title="Toggle: page break" id="viewMode-btn" class="]])
  table.insert(h, (viewMode == "tree" and "active" or ""))
  table.insert(h, [[" onclick="window.toggleViewMode()"><span id="viewMode-icon">]])
  if viewMode == "tree" then
    table.insert(h, ICONS.tree)
  else
    table.insert(h, ICONS.list)
  end
  table.insert(h, [[</span></div>
                </div>
                <div class="action-buttons">]])
  table.insert(h, [[<div  title="Toggle: attribute display"
                    class="explorer-action-btn" id="attrDisplay-btn" onclick="window.toggleAttrDisplay()">]])
  table.insert(h, ICONS.brackets)
  table.insert(h, [[</div>
                  <div title="Reset all" class="explorer-action-btn" id="refresh-btn"
                    onclick="syscall('lua.evalExpression', 'refreshExplorer2Button()')">]])
  table.insert(h, ICONS.refresh)
  table.insert(h, [[</div>
                  <div  title="Export list and print"
                    class="explorer-action-btn" id="print-btn" onclick="window.exportTaskList()">]])
  table.insert(h, ICONS.fileup)
  table.insert(h, [[</div>
                </div>
                <div class="action-buttons">
                    <div class="explorer-close-btn" title="Close Task Commander" onclick="syscall('editor.invokeCommand', 'Navigate: Task Commander')">]])
  table.insert(h, ICONS.close)
  table.insert(h, [[</div>
                </div>
              </div>]])

  local breadcrumbHtml = "<div class='explorer-breadcrumbs2' >" ..
      "<span class='titleTasks'>Tasks list</span>" ..
      "<span id='temp-message1'>update done</span>" ..
      "<span id='temp-message2'>refresh done</span>" ..
      "<span id='temp-message3'>" ..
      "<span id='statusCount'></span>" ..
      "<span id='statusInfoBtn' title='Details about the displayed list' onclick='toggleStatusPopup(event)'>&#9432;</span>" ..
      "</span>" ..
      "<div id='statusPopup' style='display:none;'>" ..
      "<div id='statusPopupContent'></div>" ..
      "</div>" ..
      "</div>"
  table.insert(h, breadcrumbHtml)
  table.insert(h, [[
              </div>]])

  local gridClass = "document-explorer document-explorer2"
  table.insert(h, [[<div class="]] .. gridClass .. [[" id="explorerGrid" "]])
  table.insert(h, [[">]])

  -- ---------- HTML ----------
  -- 1) Switch content of h into h2
  local h2 = {}
  for i = 1, #h do
    h2[i] = h[i]
  end

  -- 2) Add html provided by tasksByPage
  local stadeInit = js.window.sessionStorage.getItem("searchInit");
  if stadeInit == "true" then
    clauseWhere = WHERE_INIT
    js.window.sessionStorage.setItem("searchInit", "false");
    js.window.sessionStorage.setItem("searchTerm", WHERE_INIT)
  else
    clauseWhere = clauseWhere or ""
  end
  local listVar = tasksByPage(allTasks, viewMode, clauseWhere)
  if some(listVar) == nil then
    editor.flashNotification("Extraction failed !")
    return
  end

  -- 3) Write query to history file (except at startup)
  if stadeInit == "true" then
    stadeInit = "false"
  else
    if clauseWhere ~= "" then writeToHistory(clauseWhere) end
  end

  -- 4) Inject data-order for sort restore (before inserting into h2)
  local addHtml = listVar[1]
  local orderCounter = 0
  addHtml = addHtml:gsub(
    '(<div class="sb%-TaskExplorer%-div%-task[^"]*" data%-task%-id="[^"]*")',
    function(match)
      orderCounter = orderCounter + 1
      return match .. ' data-order="' .. string.format("%04d", orderCounter) .. '"'
    end
  )

  -- 5) Feedback
  table.insert(h2, addHtml)
  table.insert(h2, "</div>")
  local nbTasks = listVar[2]
  local finalHtml = table.concat(h2)
  -- Anti-cache: unique marker to force panel iframe reload on each drawPanel call
  local cacheBreaker = "<!-- ts:" .. tostring(os.time()) .. "." .. tostring(math.random(1000, 9999)) .. " -->"
  finalHtml = finalHtml .. cacheBreaker
  finalHtml = finalHtml:gsub("<span id='statusCount'></span>",
    "<span id='statusCount'>" .. nbTasks .. " displayed</span>")

  -- ---------- JS SCRIPT ----------
  local script = [[

    (function() {
      // 1- Hide panel while waiting for styles to fully load (Promise)
      const panel = parent.document.querySelector('.sb-panel.]] .. PANEL_ID .. [[');
      if (panel) panel.style.visibility = 'hidden';

      // 1b- Hide grid if scroll restore is pending (avoid flash before repositioning)
      const shouldRestore = sessionStorage.getItem("shouldRestore");
      if (shouldRestore === "true") {
        const container = document.getElementById("explorerGrid");
        if (container) {
          container.style.opacity = "0";
          container.style.transition = "none";
        }
      }
    })();

    (function() {
    // 2- Show a message when Draw() is finished
    ]]
  if seeMess ~= "" and seeMess ~= nil and seeMess ~= false then
    if seeMess == "maj" then
      script = script .. [[
        setTimeout(function() {
          document.getElementById('temp-message1').style.display = 'none';
          document.getElementById('temp-message2').style.display = 'inline';]]
    else
      script = script .. [[
        setTimeout(function() {
          document.getElementById('temp-message2').style.display = 'none';
          document.getElementById('temp-message1').style.display = 'inline';]]
    end
    script = script .. [[
          setTimeout(function() {
            document.getElementById('temp-message1').style.display = 'none';
            document.getElementById('temp-message2').style.display = 'none';
          }, 1300);
        }, 100);
      ]]
  end

  script = script .. [[
    // 3- Load Styles Once then reveal panel

    function ensureElement(id, tag, attributes, content) {
        if (document.getElementById(id)) return document.getElementById(id);
        const el = document.createElement(tag);
        el.id = id;
        for (let key in attributes) el.setAttribute(key, attributes[key]);
        if (content) el.innerHTML = content;
        document.head.appendChild(el);
        return el;
    }

    const mainCss = ensureElement("silverbullet-main-css", "link", { rel: "stylesheet", href: "/.client/main.css"});
    const explorerCss = ensureElement("explorer-style-css",    "link", { rel: "stylesheet", href: "/.fs/Library/baudogit/TCstyles.css" });

    if (!document.getElementById("explorer-custom-styles-once")) {
        const parentStyles = parent.document.getElementById("custom-styles")?.innerHTML || "";
        const cleanStyles = parentStyles.replace(/<\/?style>/g, "");
        const styleEl = document.createElement("style");
        styleEl.id = "explorer-custom-styles-once";
        styleEl.innerHTML = cleanStyles;
        document.head.appendChild(styleEl);
    }

    function onStyleLoaded(el, timeoutMs = 5000) {
        const { promise, resolve, reject } = Promise.withResolvers();

        if (el.sheet) {
            resolve();
            return promise;
        }

        const timeout = setTimeout(() => {
            reject(new Error(`CSS loading timeout: ${el.href}`));
        }, timeoutMs);

        const cleanup = () => clearTimeout(timeout);

        el.addEventListener('load', () => {
            cleanup();
            resolve();
        }, { once: true });

        el.addEventListener('error', (e) => {
            cleanup();
            reject(e);
        }, { once: true });

        return promise;
    }

    Promise.allSettled([
        onStyleLoaded(mainCss),
        onStyleLoaded(explorerCss)
    ]).then(results => {
        const failures = results.filter(r => r.status === 'rejected');
        if (failures.length > 0) {
            console.warn('Some CSS failed to load:', failures);
        }

        requestAnimationFrame(() => {
            document.documentElement.style.visibility = '';

        const panel = parent.document.querySelector('.sb-panel.]] .. PANEL_ID .. [[');
        if (panel) panel.style.visibility = '';
        });
    }).catch(error => {
        console.error('Critical error loading styles:', error);
        document.documentElement.style.visibility = ''; // fallback
    });

  })();

(function () {

  // !!!=========================================
  // 1. VARIABLES, CONSTANTS AND DATA
  // ============================================

  let searchMode = sessionStorage.getItem("searchMode") || "list";

  // Données (propriétés et exemples)
  const properties = [
    "ref",
    "tag",
    "name",
    "page",
    "parent",
    "pos",
    "range",
    "text",
    "state",
    "done",
    "deadline",
    "itags",
    "tags"
  ];

  // Clauses where examples. Format: "01 | clause_where | comment"
  let examples = [];
  (async function loadExamples() {
    try {
      const content = await globalThis.syscall("space.readFile", "Library/baudogit/where-examples.txt");
      const lines = new TextDecoder().decode(content).split('\n');

      for (const line of lines) {
        let trimmed = line.trim();
        if (trimmed !== '') {
          trimmed = trimmed.replace(/^["']|["'],?$/g, '');

          const firstPipeIndex = trimmed.indexOf(' | ');
          if (firstPipeIndex === -1) {
            examples.push(trimmed);
            continue;
          }

          const afterFirstPipe = trimmed.substring(firstPipeIndex + 3);
          const secondPipeIndex = afterFirstPipe.indexOf(' | ');

          if (secondPipeIndex === -1) { // no comment
            examples.push({
              clause: afterFirstPipe,
              comment: '',
              full: trimmed
            });
          } else {
            const clause = afterFirstPipe.substring(0, secondPipeIndex);
            const comment = afterFirstPipe.substring(secondPipeIndex + 3);

            examples.push({
              clause: clause,
              comment: comment,
              full: trimmed
            });
          }
        }
      }
    } catch (error) {
      console.error("Error while reading:", error);
    }
  })();

  // Clauses where history
  let history = [];
  (async function loadHistory() {
    try {
      const content = await globalThis.syscall("space.readFile", "Library/baudogit/where-history.txt");
      const lines = new TextDecoder().decode(content).split('\n');

      for (const line of lines) {
        let trimmed = line.trim();
        if (trimmed !== '') {
          trimmed = trimmed.replace(/^["']|["'],?$/g, '');
          history.push(trimmed);
        }
      }
    } catch (error) {
      console.error("Error while reading:", error);
    }
  })();

  // !!!=========================================
  // 2. GLOBAL ACTIONS (called from HTML onclick)
  // ============================================

  // Button [ ]: inject a class in body to reveal attribute structure
  window.toggleAttrDisplay = function() {
    const iframeBody = document.body;
    if (!iframeBody) return;
    iframeBody.classList.toggle('attr-alt-display');

    const btn = document.getElementById('attrDisplay-btn');
    if (btn) {
      if (iframeBody.classList.contains('attr-alt-display')) {
        btn.classList.add('active');
      } else {
        btn.classList.remove('active');
      }
    }
  };

  window.toggleViewMode = function() {
    var btn = document.getElementById('viewMode-btn');
    var icon = document.getElementById('viewMode-icon');
    if (!btn) return;

    var isTree = btn.classList.contains('active');
    var newMode = isTree ? 'list' : 'tree';

    syscall('editor.invokeCommand', 'TaskExplorer: Change View Mode', { mode: newMode });
  };

  // --- Build status context text (shared by printTasks and exportTaskList) ---
  function buildStatusText(taskCount) {
    var container = document.getElementById('explorerGrid');
    var whereClause = sessionStorage.getItem('searchTerm') || '';
    var sortActive = container ? container.getAttribute('data-sort-active') || '' : '';
    var sortFilter = container ? container.getAttribute('data-sort-filter') || '' : '';
    var filterTerm = sessionStorage.getItem('searchTermFilter') || '';

    var parts = [];
    parts.push('Where: ' + (whereClause || 'all tasks'));

    var toggleIcon = document.getElementById('tree-toggle-icon');
    if (toggleIcon) {
      var hasPlus = toggleIcon.querySelector('.icon-square-plus');
      parts.push('Completed: ' + (hasPlus ? 'included' : 'excluded'));
    }

    var viewModeBtn = document.getElementById('viewMode-btn');
    if (viewModeBtn) {
      var isTree = viewModeBtn.classList.contains('active');
      parts.push('Display: ' + (isTree ? 'page break' : 'flat list'));
    }

    if (sortActive) {
      var sortPart = 'Sort: ' + sortActive;
      if (sortFilter) sortPart += ' + Filter: ' + sortFilter;
      parts.push(sortPart);
    }
    if (filterTerm) parts.push('Filter: ' + filterTerm);
    parts.push(taskCount + ' tasks');

    var text = parts.join(' | ');

    if (window._statResultLine) {
      text += '\nStatistic: ' + window._statResultLine;
    }

    return text;
  }

  // Print: open visible tasks in a new tab for printing
  window.printTasks = function() {
    const container = document.getElementById('explorerGrid');
    if (!container) return;

    // 1. Collect visible tasks HTML
    const visibleTasks = [];
    container.querySelectorAll('.sb-TaskExplorer-div-task').forEach(function(div) {
      if (div.style.display !== 'none') {
        visibleTasks.push(div.outerHTML);
      }
    });

    if (visibleTasks.length === 0) {
      syscall('editor.flashNotification', 'No visible tasks to print');
      return;
    }

    // 2. Collect all styles from the iframe
    let allStyles = '';

    // 2a. Inline <style> tags (TCstyles loaded via ensureElement)
    document.querySelectorAll('style').forEach(function(styleEl) {
      allStyles += styleEl.outerHTML + '\n';
    });

    // 2b. <link rel="stylesheet"> tags from the iframe (convert to absolute URLs)
    document.querySelectorAll('link[rel="stylesheet"]').forEach(function(linkEl) {
      const href = linkEl.getAttribute('href');
      if (href) {
        const absoluteHref = new URL(href, document.baseURI).href;
        allStyles += '<link rel="stylesheet" href="' + absoluteHref + '">\n';
      }
    });

    // 3. Detect attr-alt-display class
    const hasAltDisplay = document.body.classList.contains('attr-alt-display');

    // 4. Build status info
    const statusText = buildStatusText(visibleTasks.length);
    const statusLines = statusText.split('\n');
    let statusHtml = '<div class="print-status">';
    statusHtml += '<strong>' + statusLines[0].substring(0, statusLines[0].indexOf(':')) + ':</strong> ' + statusLines[0].substring(statusLines[0].indexOf(':') + 2);

    // Rebuild HTML with bold labels from the plain text
    (function() {
      var raw = buildStatusText(visibleTasks.length);
      var lines = raw.split('\n');
      var firstLine = lines[0];
      // Split first line by " | " and bold each label
      var segments = firstLine.split(' | ');
      statusHtml = '<div class="print-status">';
      segments.forEach(function(seg, i) {
        if (i > 0) statusHtml += ' &nbsp;|&nbsp; ';
        var colonIdx = seg.indexOf(':');
        if (colonIdx !== -1) {
          statusHtml += '<strong>' + seg.substring(0, colonIdx + 1) + '</strong>' + seg.substring(colonIdx + 1);
        } else {
          statusHtml += '<strong>' + seg + '</strong>';
        }
      });
      if (lines.length > 1) {
        statusHtml += '<br><strong>' + lines[1].substring(0, lines[1].indexOf(':') + 1) + '</strong>' + lines[1].substring(lines[1].indexOf(':') + 1);
      }
      statusHtml += '</div>';
    })();

    // 5. Build the print page
    const baseHref = window.location.origin + '/';
    const printHTML = '<!DOCTYPE html>\n<html>\n<head>\n' +
      '<meta charset="UTF-8">\n' +
      '<base href="' + baseHref + '">\n' +
      '<title>Task Commander — Print</title>\n' +
      allStyles + '\n' +
      '<style>\n' +
      '  @media print {\n' +
      '    .print-status { page-break-after: avoid; }\n' +
      '    .print-container { page-break-inside: auto; }\n' +
      '    .sb-TaskExplorer-div-task { page-break-inside: avoid; }\n' +
      '  }\n' +
      '  body { padding: 10px 20px; overflow-y: auto; height: auto; }\n' +
      '  html { height: auto; overflow-y: auto; }\n' +
      '  .print-status {\n' +
      '    font-size: 12px; color: #666; border-bottom: 1px solid #ccc;\n' +
      '    padding: 5px 0 8px 0; margin-bottom: 10px;\n' +
      '  }\n' +
      '  .print-container {\n' +
      '    display: flex; flex-direction: column;\n' +
      '  }\n' +
      '</style>\n' +
      '</head>\n' +
      '<body' + (hasAltDisplay ? ' class="attr-alt-display"' : '') + '>\n' +
      statusHtml + '\n' +
      '<div class="print-container" id="explorerGrid">\n' +
      visibleTasks.join('\n') + '\n' +
      '</div>\n' +
      '</body>\n</html>';

    // 6. Open in new tab and trigger print
    const printWindow = window.open('', '_blank');
    if (!printWindow) {
      syscall('editor.flashNotification', 'Popup blocked — please allow popups for this site');
      return;
    }
    printWindow.document.write(printHTML);
    printWindow.document.close();

    // Wait for styles and content to load before printing
    printWindow.onload = function() {
      setTimeout(function() {
        printWindow.print();
      }, 300);
    };
  };

  // --- Button Export: modal choice HTML / MD (with optional stat results) ---
  window.exportTaskList = function(statResultLine) {
    var parentDoc = parent.document;

    var existing = parentDoc.getElementById('exportChoiceModal');
    if (existing) existing.remove();

    // Build stat block if stat results are provided
    var statBlock = '';
    if (statResultLine) {
      statBlock = '<div style="background: #f5f5f5; padding: 12px 15px; border-radius: 4px;' +
        'font-family: monospace; font-size: 14px; line-height: 1.6; margin: 0 0 15px 0;' +
        'word-break: break-word;">' + statResultLine + '</div>';
    }

    var modalHTML = '<div id="exportChoiceModal" style="' +
      'position: fixed; top: 0; left: 0; width: 100%; height: 100%;' +
      'background-color: rgba(0, 0, 0, 0.5); display: flex;' +
      'justify-content: center; align-items: center; z-index: 10000;">' +
      '<div style="background: white; border-radius: 8px; padding: 20px;' +
      'max-width: 500px; min-width: 280px; box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);">' +
      '<h3 style="margin: 0 0 15px 0; color: #333;">Export task list</h3>' +
      statBlock +
      '<div style="margin-bottom: 20px; color: #555;">Choose the export format:</div>' +
      '<div style="display: flex; gap: 10px; justify-content: flex-end;">' +
      '<button autofocus type="button" id="exportCancel" style="' +
      'padding: 8px 20px; border: 1px solid #ccc; background: white;' +
      'border-radius: 4px; cursor: pointer; font-size: 14px;">Cancel</button>' +
      '<button type="button" id="exportHtml" style="' +
      'padding: 8px 20px; border: none; background: #2563eb; color: white;' +
      'border-radius: 4px; cursor: pointer; font-size: 14px; font-weight: 500;">Export HTML</button>' +
      '<button type="button" id="exportMd" style="' +
      'padding: 8px 20px; border: none; background: #059669; color: white;' +
      'border-radius: 4px; cursor: pointer; font-size: 14px; font-weight: 500;">Export MD</button>' +
      '</div></div></div>';

    parentDoc.body.insertAdjacentHTML('beforeend', modalHTML);

    var modal = parentDoc.getElementById('exportChoiceModal');
    var btnCancel = parentDoc.getElementById('exportCancel');
    var btnHtml = parentDoc.getElementById('exportHtml');
    var btnMd = parentDoc.getElementById('exportMd');

    btnCancel.focus();
    setTimeout(function() {
      var el = parentDoc.getElementById('exportCancel');
      if (el) el.focus();
    }, 0);

    btnCancel.onclick = function() {
      modal.remove();
    };

    btnHtml.onclick = function() {
      modal.remove();
      if (statResultLine) {
        window._statResultLine = statResultLine;
      }
      window.printTasks();
      window._statResultLine = null;
    };

    btnMd.onclick = function() {
      modal.remove();

      // Collect visible task IDs
      var taskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');
      var taskIds = [];
      taskDivs.forEach(function(div) {
        if (window.getComputedStyle(div).display !== 'none') {
          var taskId = div.getAttribute('data-task-id');
          if (taskId) {
            taskIds.push(taskId);
          }
        }
      });

      if (taskIds.length === 0) {
        syscall('editor.flashNotification', 'No visible tasks to export');
        return;
      }

      // Build context text (same content as print status)
      if (statResultLine) {
        window._statResultLine = statResultLine;
      }
      var ch = buildStatusText(taskIds.length);
      window._statResultLine = null;

      // Call Lua command with task IDs and context
      syscall('editor.invokeCommand', 'TaskExplorer: Export task list', [taskIds, ch]);
    };
  };

  // !!!=========================================
  // 3. STATUS BAR AND UTILITIES
  // ============================================

  function updateStatusBar() {
    const allTasks = document.querySelectorAll('.sb-TaskExplorer-div-task');
    let visibleCount = 0;
    allTasks.forEach(function(div) {
      if (div.style.display !== 'none') visibleCount++;
    });


    const countSpan = document.getElementById('statusCount');
    if (countSpan) {
      countSpan.textContent = visibleCount + ' displayed';
    }

    const popupContent = document.getElementById('statusPopupContent');
    if (popupContent) {
      const whereClause = sessionStorage.getItem('searchTerm') || '';
      const filterTerm = sessionStorage.getItem('searchTermFilter') || '';
      const container = document.getElementById('explorerGrid');
      const sortActive = container ? container.getAttribute('data-sort-active') || '' : '';
      const sortFilter = container ? container.getAttribute('data-sort-filter') || '' : '';

      let html = '';
      html += '<div class="status-detail-row"><b>Where:</b> ' + (whereClause || '<em>all the tasks</em>') + '</div>';

      // Implicit filters from toolbar buttons
      var toggleIcon = document.getElementById('tree-toggle-icon');
      if (toggleIcon) {
        var hasPlus = toggleIcon.querySelector('.icon-square-plus');
        html += '<div class="status-detail-row"><b>Completed:</b> ' + (hasPlus ? 'included' : 'excluded') + '</div>';
      }

      var viewModeBtn = document.getElementById('viewMode-btn');
      if (viewModeBtn) {
        var isTree = viewModeBtn.classList.contains('active');
        html += '<div class="status-detail-row"><b>Display:</b> ' + (isTree ? 'page break' : 'flat list') + '</div>';
      }

      if (sortActive) {
        html += '<div class="status-detail-row"><b>Sort:</b> ' + sortActive + '</div>';
      }
      if (sortFilter) {
        html += '<div class="status-detail-row"><b>Sort filter:</b> ' + sortFilter + '</div>';
      }
      if (filterTerm) {
        html += '<div class="status-detail-row"><b>Filter:</b> ' + filterTerm + '</div>';
      }
      popupContent.innerHTML = html;
    }
  }

  // --- Display popup ---
  window.toggleStatusPopup = function(e) {
    if (e) { e.stopPropagation(); }
    const popup = document.getElementById('statusPopup');
    if (!popup) return;

    if (popup.style.display === 'none' || popup.style.display === '') {
      updateStatusBar();
      popup.style.display = 'block';

      setTimeout(function() {
        document.addEventListener('click', function closePopup(ev) {
          if (!popup.contains(ev.target) && ev.target.id !== 'statusInfoBtn') {
            popup.style.display = 'none';
            document.removeEventListener('click', closePopup);
          }
        });
      }, 0);
    } else {
      popup.style.display = 'none';
    }
  };

  // --- Filter/Sort state detection and Clear button ---

  // Detect whether a filter action is currently active (list mode)
  function isFilterActive() {
    const filterTerm = sessionStorage.getItem('searchTermFilter') || '';
    return filterTerm !== '';
  }

  // Detect whether a sort action is currently active (sort mode)
  function isSortActive() {
    const container = document.getElementById('explorerGrid');
    if (!container) return false;
    return !!(container.getAttribute('data-sort-active') || container.getAttribute('data-sort-filter'));
  }

  // Update clear button visibility: × shown only if
  // - input has content, OR
  // - current mode is list AND a filter is active, OR
  // - current mode is sort AND a sort is active
  function updateClearBtnVisibility(inputValue) {
    const clearBtn = document.getElementById('inputClearBtn');
    if (!clearBtn) return;
    const hasContent = !!inputValue;
    const modeAction = (searchMode === 'list' && isFilterActive())
                    || (searchMode === 'sort' && isSortActive());
    clearBtn.style.visibility = (hasContent || modeAction) ? 'visible' : 'hidden';
  }

  // Clear function
  window.clearSearchInput = function() {
    const input = document.getElementById('tileSearch');
    if (!input) return;
    input.value = '';
    input.focus();

    if (searchMode === 'list') {
      sessionStorage.setItem('searchTermFilter', '');
      applySearchFilter('');
      const btn = document.querySelector('#listMode');
      if (btn) { btn.title = 'Filter mode'; }
      // Also clear sort-filter if it was active
      const container = document.getElementById('explorerGrid');
      if (container) { container.removeAttribute('data-sort-filter'); }
    } else if (searchMode === 'sort') {
      const container = document.getElementById('explorerGrid');
      const sortActive = container ? container.getAttribute('data-sort-active') : null;
      if (sortActive) {
        clearSortState();
      }
    }

    updateClearBtnVisibility('');
  };

  // Prevent the clear button from stealing focus
  (function() {
    const clearBtn = document.getElementById('inputClearBtn');
    if (clearBtn) {
      clearBtn.addEventListener('mousedown', function(e) {
        e.preventDefault();
      });
    }
  })();

  // !!!=========================================
  // 4. TASK STATUS TOGGLE
  // ============================================
  window.toggleTask = function (namePage, positionStart, done, status) {
    const taskId = namePage + "@" + (positionStart - 2);
    const clickedElement = document.querySelector(
      '[data-task-id="' + taskId + '"]',
    );

    sessionStorage.setItem("lastTaskId", taskId);
    sessionStorage.setItem("shouldRestore", "true");

    syscall("editor.invokeCommand", "Task: Toggle_Done", [
      namePage,
      positionStart,
      done,
      status,
    ]);
  };

  // The updated row is positioned at the top of the list
  function restoreScrollPosition() {
    const shouldRestore = sessionStorage.getItem("shouldRestore");

    if (shouldRestore === "true") {
      const lastTaskId = sessionStorage.getItem("lastTaskId");

      if (lastTaskId) {
        const taskElement = document.querySelector(
          '[data-task-id="' + lastTaskId + '"]',
        );

        if (taskElement) {
          taskElement.scrollIntoView({
            behavior: "instant",
            block: "start",
          });
        }
      }

      setTimeout(function () {
        const container = document.getElementById("explorerGrid");
        if (container) {
          container.style.opacity = "1";
        }
        sessionStorage.removeItem("shouldRestore");
      }, 200);
    }
  }

  // !!!=========================================
  // 5. FILTER — Apply, Parse, Match
  // ============================================
  function applySearchFilter(searchTermFilter) {
    searchTermFilter = searchTermFilter.trim();
    const allTasks = document.querySelectorAll(".sb-TaskExplorer-div-task");

    // Parse search terms with support for brackets, quotes, + and - operators
    const filterRules = parseSearchTerms(searchTermFilter);

    let nbTasks = 0;
    allTasks.forEach(function (task) {
      const labelDiv = task.querySelector("#divTaskchild");
      const postit = labelDiv.getAttribute('title') || '';

      if (labelDiv) {
        const labelText = labelDiv.textContent;

        // If no search term, show all
        if (filterRules.orGroups.length === 0 && filterRules.notTerms.length === 0) {
          task.style.display = "";
        } else {
          // First check negations (NOT terms)
          const hasExcludedTerm = filterRules.notTerms.some(function (term) {
            return matchTerm(term, labelText, postit);
          });

          if (hasExcludedTerm) {
            // If an excluded term is found, hide the task
            task.style.display = "none";
          } else {
            // Then check positive filters (OR groups)
            if (filterRules.orGroups.length === 0) {
              // No positive filter, show the task (only negations were applied)
              task.style.display = "";
            } else {
              // Check that at least one group (OR) is satisfied
              const anyGroupMatches = filterRules.orGroups.some(function (andGroup) {
                // For a group to be satisfied, all its terms (AND) must be found
                return andGroup.every(function (term) {
                  return matchTerm(term, labelText, postit);
                });
              });

              if (anyGroupMatches) {
                task.style.display = "";
              } else {
                task.style.display = "none";
              }
            }
          }
        }
        if (task.style.display !== "none") {nbTasks++; };
      }
    });
    // Hide page titles without visible tasks
    const allPageHeaders = document.querySelectorAll(
      ".sb-TaskExplorer-div-page",
    );
    allPageHeaders.forEach(function (header) {
      let sibling = header.nextElementSibling;
      let hasVisibleTasks = false;

      while (
        sibling &&
        !sibling.classList.contains("sb-TaskExplorer-div-page")
      ) {
        if (
          sibling.classList.contains("sb-TaskExplorer-div-task") &&
          sibling.style.display !== "none"
        ) {
          hasVisibleTasks = true;
          break;
        }
        sibling = sibling.nextElementSibling;
      }

      header.style.display = hasVisibleTasks ? "" : "none";
    });

    // Centralized status bar update
    updateStatusBar();
  }

  // --- Parse search terms ---
  function parseSearchTerms(searchTermFilter) {
    // Separate positive filters (OR/AND groups) and negative filters (NOT terms)
    const orGroups = [];
    const notTerms = [];
    let currentGroup = [];
    let i = 0;

    while (i < searchTermFilter.length) {
      const char = searchTermFilter[i];

      // Ignore spaces at the beginning
      if (char === ' ') {
        // If we have a group in progress, we end it (OR operator)
        if (currentGroup.length > 0) {
          orGroups.push(currentGroup);
          currentGroup = [];
        }
        i++;
        continue;
      }

      // Operator +: continues the current group (AND operator)
      if (char === '+') {
        i++;
        continue;
      }

      // Operator -: negation (NOT operator)
      // BUT only if it's at the beginning OR preceded by a space/operator
      if (char === '-') {
        // Check if it is really an operator (start of term)
        const isOperator = (i === 0 ||
                            searchTermFilter[i - 1] === ' ' ||
                            searchTermFilter[i - 1] === '+');

        if (isOperator) {
          i++; // Skip the '-'
          const term = parseSingleTerm(searchTermFilter, i);
          if (term && term.term !== null) {
            notTerms.push(term.term);
            i = term.nextIndex;
          }
          continue;
        }
        // Otherwise, it's a normal character, we let parseSingleTerm handle it
      }

      // Parse a positive term
      const term = parseSingleTerm(searchTermFilter, i);
      if (term) {
        // Add only valid (non-null) terms
        if (term.term !== null) {
          currentGroup.push(term.term);
        }
        i = term.nextIndex;
      } else {
        i++;
      }
    }

    // Add the last group if it exists
    if (currentGroup.length > 0) {
      orGroups.push(currentGroup);
    }

    return {
      orGroups: orGroups,
      notTerms: notTerms
    };
  }

  // --- Parse a single term ---
  function parseSingleTerm(searchTermFilter, startIndex) {
    let i = startIndex;
    const char = searchTermFilter[i];

    // Detect itags (#word)
    if (char === '#') {
      i++; // Skip the '#'
      let word = '';
      while (i < searchTermFilter.length &&
              searchTermFilter[i] !== ' ') {
        word += searchTermFilter[i];
        i++;
      }

      if (word.length > 0) {
        return {
          term: { type: 'itag', value: word },
          nextIndex: i
        };
      }
    }

    // Block in curly braces {...} — AND group of itags
    if (char === '{') {
      const closingIndex = searchTermFilter.indexOf('}', i);
      if (closingIndex !== -1) {
        const content = searchTermFilter.substring(i + 1, closingIndex);
        // Parse all #tags inside the braces
        const tags = [];
        const tagRegex = /#(\S+)/g;
        let match;
        while ((match = tagRegex.exec(content)) !== null) {
          tags.push(match[1]);
        }
        if (tags.length > 0) {
          return {
            term: { type: 'itagGroup', value: tags },
            nextIndex: closingIndex + 1
          };
        }
        // No #tags found: treat the content as a normal text search
        if (content.trim().length > 0) {
          return {
            term: { type: 'normal', value: content.trim() },
            nextIndex: closingIndex + 1
          };
        }
        return { term: null, nextIndex: closingIndex + 1 };
      } else {
        // No closing brace found, skip
        return { term: null, nextIndex: i + 1 };
      }
    }

    // Block in double square brackets - search in postit (task ID)
    var dblOpen = String.fromCharCode(91, 91);
    var dblClose = String.fromCharCode(93, 93);
    if (char === '[' && i + 1 < searchTermFilter.length && searchTermFilter[i + 1] === '[') {
      var closingIndex = searchTermFilter.indexOf(dblClose, i + 2);
      if (closingIndex !== -1) {
        var content = searchTermFilter.substring(i + 2, closingIndex);
        return {
          term: { type: 'taskId', value: content },
          nextIndex: closingIndex + 2
        };
      } else {
        return { term: null, nextIndex: i + 1 };
      }
    }

    // Block in square brackets [...]
    if (char === '[') {
      const closingIndex = searchTermFilter.indexOf(']', i);
      if (closingIndex !== -1) {
        const content = searchTermFilter.substring(i + 1, closingIndex);
        return {
          term: { type: 'bracket', value: content },
          nextIndex: closingIndex + 1
        };
      } else {
        // No closing bracket found, we don't know
        return { term: null, nextIndex: i + 1 };
      }
    }

    // Block in double quotes "..."
    if (char === '"') {
      const closingIndex = searchTermFilter.indexOf('"', i + 1);
      if (closingIndex !== -1) {
        const content = searchTermFilter.substring(i + 1, closingIndex);
        return {
          term: { type: 'quoted', value: content },
          nextIndex: closingIndex + 1
        };
      } else {
        // No closing quote found, we ignore
        return { term: null, nextIndex: i + 1 };
      }
    }

    // Block in single quotes '...'
    if (char === "'") {
      const closingIndex = searchTermFilter.indexOf("'", i + 1);
      if (closingIndex !== -1) {
        const content = searchTermFilter.substring(i + 1, closingIndex);
        return {
          term: { type: 'quoted', value: content },
          nextIndex: closingIndex + 1
        };
      } else {
        // No closing quote found, we ignore
        return { term: null, nextIndex: i + 1 };
      }
    }

    // Normal word (without special delimiter)
    let word = '';
    while (i < searchTermFilter.length &&
            searchTermFilter[i] !== ' ' &&
            searchTermFilter[i] !== '+' &&
            searchTermFilter[i] !== '-' &&
            searchTermFilter[i] !== '[' &&
            searchTermFilter[i] !== '{' &&
            searchTermFilter[i] !== '"' &&
            searchTermFilter[i] !== "'") {
      word += searchTermFilter[i];
      i++;
    }

    if (word.length > 0) {
      return {
        term: { type: 'normal', value: word },
        nextIndex: i
      };
    }

    return null;
  }

  // --- Match a term against text ---
  function matchTerm(term, text, postit) {
    if (term.type === 'itag') {
      if (!postit) return false;

      const itagsMatch = postit.match(/ITAGS:\s*\{([^}]+)\}/);
      if (!itagsMatch) return false;

      const itagsList = itagsMatch[1].split(',').map(function(tag) {
        return tag.trim();
      });

      const searchValue = term.value.toLowerCase();
      return itagsList.some(function(tag) {
        return tag.toLowerCase() === searchValue;
      });
    }

    if (term.type === 'itagGroup') {
      if (!postit) return false;

      const itagsMatch = postit.match(/ITAGS:\s*\{([^}]+)\}/);
      if (!itagsMatch) return false;

      const itagsList = itagsMatch[1].split(',').map(function(tag) {
        return tag.trim().toLowerCase();
      });

      return term.value.every(function(searchTag) {
        const searchValue = searchTag.toLowerCase();
        return itagsList.some(function(tag) {
          return tag === searchValue;
        });
      });
    }

    if (term.type === 'taskId') {
      if (!postit) return false;
      return postit.toLowerCase().indexOf(term.value.toLowerCase()) !== -1;
    }

    if (term.type === 'bracket') {
      // For brackets: look for the string that starts just after a [
      // Additional characters can follow, but not precede
      const bracketBlocks = [];
      let inBracket = false;
      let currentBlock = '';

      for (let i = 0; i < text.length; i++) {
        if (text[i] === '[') {
          if (inBracket && currentBlock) {
            bracketBlocks.push(currentBlock);
          }
          inBracket = true;
          currentBlock = '';
        } else if (text[i] === ']' && inBracket) {
          if (currentBlock) {
            bracketBlocks.push(currentBlock);
          }
          inBracket = false;
          currentBlock = '';
        } else if (inBracket) {
          currentBlock += text[i];
        }
      }

      const searchValue = term.value.toLowerCase();
      // Check that the block STARTS with the desired pattern
      // Quotes inside brackets are neutralized (stripped before comparison)
      return bracketBlocks.some(function(block) {
        return block.replace(/['"]/g, '').toLowerCase().startsWith(searchValue);
      });
    }
    else if (term.type === 'quoted') {
      return text.toLowerCase().includes(term.value.toLowerCase());
    }
    else {
      return text.toLowerCase().includes(term.value.toLowerCase());
    }
  }

  // !!!=========================================
  // 6. SORT — Parse, Extract, Compare, Apply, Clear
  // ============================================

  function parseSortCriteria(criteria) {
    const parts = criteria.trim().split(/\s+/);
    const sortKeys = [];

    for (let part of parts) {
      if (!part) continue;

      const desc = part.endsWith('-');
      const asc = part.endsWith('+');

      if (!desc && !asc) {
        // No suffix, default to ascending
        sortKeys.push({ attr: part, desc: false });
      } else {
        const attr = part.slice(0, -1); // Remove last character (+ or -)
        sortKeys.push({ attr: attr, desc: desc });
      }
    }

    return sortKeys;
  }

  // Extract attribute value from a task div
  function extractAttributeValue(taskDiv, attrName) {
    const textContent = taskDiv.textContent || taskDiv.innerText || "";

    const searchStr = '[' + attrName + ':';
    const startIdx = textContent.indexOf(searchStr);

    if (startIdx === -1) {
      const dataAttr = taskDiv.getAttribute('data-' + attrName);
      return dataAttr ? dataAttr.trim() : null;
    }

    const valueStart = startIdx + searchStr.length;
    const closingIdx = textContent.indexOf(']', valueStart);

    if (closingIdx === -1) return null;

    // Extract, trim, and strip any surrounding quotes
    const value = textContent.substring(valueStart, closingIdx).trim().replace(/['"]/g, '');
    return value || null;
  }

  /**
    * Normalize date string to timestamp for consistent comparison
    * Supports:
    * - ISO format: YYYY-MM-DD, YYYY-MM-DDTHH:mm:ss
    * - French format: DD/MM/YYYY, DD-MM-YYYY, DD.MM.YYYY
    * - US format: MM/DD/YYYY (when day <= 12, ambiguous)
    * - Timestamps: numbers
    *
    * Returns: timestamp (number) or null if invalid
    */
  function normalizeDate(dateStr) {
    if (!dateStr) return null;

    const asNumber = parseFloat(dateStr);
    if (!isNaN(asNumber) && asNumber > 1000000000) {
      return asNumber;
    }

    // Try standard Date parsing first (handles ISO, US formats)
    let date = new Date(dateStr);
    if (!isNaN(date.getTime())) {
      return date.getTime();
    }

    // Try to detect and convert DD/MM/YYYY or DD-MM-YYYY or DD.MM.YYYY
    const europeanMatch = dateStr.match(/^(\d{1,2})[\/\-\.](\d{1,2})[\/\-\.](\d{4})$/);
    if (europeanMatch) {
      const part1 = parseInt(europeanMatch[1], 10);
      const part2 = parseInt(europeanMatch[2], 10);
      const year = parseInt(europeanMatch[3], 10);

      // If first part > 12, it must be day (DD/MM/YYYY format)
      if (part1 > 12) {
        const day = part1;
        const month = part2;
        date = new Date(year, month - 1, day);
        if (!isNaN(date.getTime())) {
          return date.getTime();
        }
      }
      // If second part > 12, it must be MM/DD/YYYY format
      else if (part2 > 12) {
        const month = part1;
        const day = part2;
        date = new Date(year, month - 1, day);
        if (!isNaN(date.getTime())) {
          return date.getTime();
        }
      }
      // Ambiguous case (both <= 12): assume DD/MM/YYYY (French default)
      else {
        const day = part1;
        const month = part2;
        date = new Date(year, month - 1, day);
        if (!isNaN(date.getTime())) {
          return date.getTime();
        }
      }
    }
    return null;
  }

  function compareValues(a, b) {
    // Handle null/undefined
    if (a === null || a === undefined) {
      if (b === null || b === undefined) return 0;
      return 1; // null values go to the end
    }
    if (b === null || b === undefined) return -1;

    // Try to parse as dates (with normalization for multiple formats)
    const timeA = normalizeDate(a);
    const timeB = normalizeDate(b);

    if (timeA !== null && timeB !== null) {
      return timeA - timeB;
    }

    // If only one is a valid date, the non-date goes to the end
    if (timeA !== null) return -1;
    if (timeB !== null) return 1;

    // Try to parse as numbers (for non-date numeric values)
    const numA = parseFloat(a);
    const numB = parseFloat(b);
    if (!isNaN(numA) && !isNaN(numB)) {
      return numA - numB;
    }

    // Text comparison (case-insensitive)
    const strA = String(a).toLowerCase();
    const strB = String(b).toLowerCase();
    return strA.localeCompare(strB);
  }

  function applySortToTasks(criteria) {
    // STEP 0: Filter is ON by default; detect " !" to disable it
    let filterAfterSort = true;
    const noFilterMatch = criteria.match(/^(.+?)\s+!\s*$/);
    if (noFilterMatch) {
      criteria = noFilterMatch[1].trim();
      filterAfterSort = false;
    }

    // STEP 0b: Show all tasks (clear any active filter and previous sort-filter)
    const allTaskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');
    allTaskDivs.forEach(div => {
      div.style.display = '';
    });

    const containerEarly = document.getElementById("explorerGrid");
    if (containerEarly) {
      containerEarly.removeAttribute('data-sort-filter');
    }

    sessionStorage.setItem("searchTermFilter", "");
    const filterBtn = document.querySelector('#listMode');
    if (filterBtn) {
      filterBtn.title = "Filter mode";
    }

    // STEP 1: Parse sort criteria
    const sortKeys = parseSortCriteria(criteria);

    if (sortKeys.length === 0) {
      console.warn("No valid sort criteria");
      return;
    }

    // STEP 2: Get all task divs as array
    const taskDivs = Array.from(allTaskDivs);

    if (taskDivs.length === 0) {
      console.warn("No tasks to sort");
      return;
    }

    // STEP 3: Extract sort values for each task
    taskDivs.forEach(div => {
      div._sortValues = {};
      sortKeys.forEach(key => {
        div._sortValues[key.attr] = extractAttributeValue(div, key.attr);
      });
    });

    // STEP 4: Sort the array
    taskDivs.sort((a, b) => {
      for (let key of sortKeys) {
        const valA = a._sortValues[key.attr];
        const valB = b._sortValues[key.attr];
        const comparison = compareValues(valA, valB);

        if (comparison !== 0) {
          return key.desc ? -comparison : comparison;
        }
      }
      return 0; // Equal on all keys
    });

    // STEP 5: Reorder DOM
    const container = document.getElementById("explorerGrid");
    if (!container) {
      console.error("Container #explorerGrid not found");
      return;
    }

    // Clear ALL (tasks + page divs)
    while (container.firstChild) {
      container.removeChild(container.firstChild);
    }

    // Re-append ONLY sorted tasks (page divs discarded)
    taskDivs.forEach(function(div) {
      container.appendChild(div);
    });

    // STEP 5b: Sync view-switcher button (sort removes page breaks = list mode visually)
    const viewModeBtn = document.getElementById('viewMode-btn');
    if (viewModeBtn) {
      viewModeBtn.classList.remove('active');
      var viewIcon = document.getElementById('viewMode-icon');
      if (viewIcon) {
        viewIcon.innerHTML = "<svg class='icon-list taskex-icon'><use href='/.fs/Library/baudogit/TCicons.svg#icon-list'></use></svg>";
      }
    }

    taskDivs.forEach(div => {
      delete div._sortValues; // cleanup
    });

    // STEP 6: Mark sort as active and save criteria
    container.setAttribute('data-sort-active', criteria);
    sessionStorage.setItem('sortCriteria', criteria);

    // STEP 7: Apply silent filter (default) unless disabled with " !"
    if (filterAfterSort) {
      // Build filter string from sort attribute names: [attr1: ][attr2: ]
      const silentFilter = sortKeys.map(function(key) {
        return '[' + key.attr + ': ]';
      }).join('');
      applySearchFilter(silentFilter);
      container.setAttribute('data-sort-filter', silentFilter);
    }

    // STEP 8: Update status bar and feedback
    updateStatusBar();
    const filterInfo = filterAfterSort ? ' (filtered)' : '';
    syscall('editor.flashNotification', 'Sorted ' + taskDivs.length + ' tasks by: ' + criteria + filterInfo);
    //console.log(`Sorted ${taskDivs.length} tasks by: ${criteria}${filterInfo}`);
  }

  // Clear sort state: restore original order using data-order
  function clearSortState() {
    const container = document.getElementById("explorerGrid");
    if (!container) return;

    container.removeAttribute('data-sort-active');
    container.removeAttribute('data-sort-filter');
    sessionStorage.removeItem('sortCriteria');

    // Restore original DOM order using data-order
    const allChildren = Array.from(container.children);
    const hasSortableItems = allChildren.some(function(el) {
      return el.hasAttribute('data-order');
    });
    if (!hasSortableItems) return;

    // Separate page divs and task divs
    const pageDivs = [];
    const taskDivs = [];
    allChildren.forEach(function(el) {
      if (el.classList.contains('sb-TaskExplorer-div-page')) {
        pageDivs.push(el);
      } else if (el.hasAttribute('data-order')) {
        taskDivs.push(el);
      }
    });

    // Sort tasks by data-order (lexicographic on 4-digit strings = correct)
    taskDivs.sort(function(a, b) {
      return a.getAttribute('data-order').localeCompare(b.getAttribute('data-order'));
    });

    // Clear container and re-append in original order
    while (container.firstChild) {
      container.removeChild(container.firstChild);
    }

    // Re-insert page divs + their tasks in original order
    // Strategy: tasks carry data-task-id with pageName@pos
    // Page divs carry their page reference — rebuild page-then-tasks structure
    if (pageDivs.length > 0) {
      // Build a map: pageName -> page div
      const pageMap = {};
      pageDivs.forEach(function(pd) {
        // Extract page name from the page div (data attribute or link)
        const link = pd.querySelector('a, [data-page]');
        const pageName = link
          ? (link.getAttribute('data-page') || link.getAttribute('href') || link.textContent.trim())
          : pd.textContent.trim();
        pageMap[pageName] = pd;
      });

      // Group tasks by page (from data-task-id = "pageName@pos")
      const tasksByPage = {};
      const pageOrder = [];
      taskDivs.forEach(function(td) {
        const taskId = td.getAttribute('data-task-id') || '';
        const atIndex = taskId.lastIndexOf('@');
        const pageName = atIndex > 0 ? taskId.substring(0, atIndex) : '';
        if (!tasksByPage[pageName]) {
          tasksByPage[pageName] = [];
          pageOrder.push(pageName);
        }
        tasksByPage[pageName].push(td);
      });

      // Re-append: for each page in task order, append page div then its tasks
      pageOrder.forEach(function(pageName) {
        // Find and append page div
        for (var key in pageMap) {
          if (pageName.indexOf(key) !== -1 || key.indexOf(pageName) !== -1) {
            container.appendChild(pageMap[key]);
            delete pageMap[key];
            break;
          }
        }
        // Append tasks for this page
        tasksByPage[pageName].forEach(function(td) {
          container.appendChild(td);
        });
      });

      // Append any remaining page divs not matched
      for (var key in pageMap) {
        container.appendChild(pageMap[key]);
      }
    } else {
      // No page divs (list mode without page breaks) — just re-append sorted tasks
      taskDivs.forEach(function(td) {
        container.appendChild(td);
      });
    }

    // Show all tasks (in case filter was applied)
    taskDivs.forEach(function(td) {
      td.style.display = '';
    });
    pageDivs.forEach(function(pd) {
      pd.style.display = '';
    });

    // Clear any list filter that was applied on top of the sort
    sessionStorage.setItem('searchTermFilter', '');

    updateStatusBar();
  }

  // !!!=========================================
  // 6b. CALC — Parse command and execute calculation
  // ============================================

  // Parse "term1 [+|-] term2 [+|-] term3 ... -> resultAttr"
  // Each term is an attribute name or a literal number
  // Returns { terms: [{value, isLiteral}], operators: ['+'/'-'], resultAttr } or null
  function parseCalcCommand(command) {
    // Split on "->"
    const arrowIdx = command.indexOf('->');
    if (arrowIdx === -1) return null;

    const leftPart = command.substring(0, arrowIdx).trim();
    const resultAttr = command.substring(arrowIdx + 2).trim();
    if (!resultAttr || !leftPart) return null;

    // Tokenize: split on " + " and " - " while keeping the operators
    // We split carefully: operators are surrounded by spaces
    const tokens = [];
    const operators = [];
    let remaining = leftPart;

    while (remaining.length > 0) {
      // Find the next operator (earliest " + " or " - ")
      let plusIdx = remaining.indexOf(' + ');
      let minusIdx = remaining.indexOf(' - ');

      let opIdx = -1;
      let op = null;

      if (plusIdx !== -1 && (minusIdx === -1 || plusIdx < minusIdx)) {
        opIdx = plusIdx;
        op = '+';
      } else if (minusIdx !== -1) {
        opIdx = minusIdx;
        op = '-';
      }

      if (opIdx === -1) {
        // No more operators, rest is the last term
        const term = remaining.trim();
        if (!term) return null;
        tokens.push(term);
        break;
      } else {
        const term = remaining.substring(0, opIdx).trim();
        if (!term) return null;
        tokens.push(term);
        operators.push(op);
        remaining = remaining.substring(opIdx + 3); // skip " + " or " - "
      }
    }

    if (tokens.length < 2 || operators.length !== tokens.length - 1) return null;

    // Build terms array
    const terms = tokens.map(function(t) {
      return {
        value: t,
        isLiteral: /^-?\d+(\.\d+)?$/.test(t)
      };
    });

    return { terms: terms, operators: operators, resultAttr: resultAttr };
  }

  // Attempt to parse a value as a date. Delegates to normalizeDate()
  // which supports ISO, French (DD/MM/YYYY), US, and timestamp formats.
  // Returns Date object (date only, no time) or null.
  function tryParseDate(val) {
    if (!val || typeof val !== 'string') return null;
    // Reject pure numbers (they should be handled by tryParseNumber)
    if (/^\s*-?\d+(\.\d+)?\s*$/.test(val)) return null;
    var ts = normalizeDate(val);
    if (ts === null) return null;
    var d = new Date(ts);
    // Strip time part: keep date only
    return new Date(d.getFullYear(), d.getMonth(), d.getDate());
  }

  // Attempt to parse a value as a number. Returns number or null.
  function tryParseNumber(val) {
    if (!val || typeof val !== 'string') return null;
    const trimmed = val.trim();
    if (trimmed === '') return null;
    const n = Number(trimmed);
    if (isNaN(n)) return null;
    return n;
  }

  // Format a Date object as YYYY-MM-DD
  function formatDate(d) {
    const y = d.getFullYear();
    const m = String(d.getMonth() + 1).padStart(2, '0');
    const day = String(d.getDate()).padStart(2, '0');
    return y + '-' + m + '-' + day;
  }

  // Compute one step: val1 op val2
  //   date - date   -> number (days)
  //   date +/- number -> date (YYYY-MM-DD)
  //   number +/- number -> number (integer)
  function computeStep(val1, val2, operator) {
    // val1 can be a string ("2026-01-01", "42") or already a result (string)
    var v1str = String(val1);
    var v2str = String(val2);

    const d1 = tryParseDate(v1str);
    const d2 = tryParseDate(v2str);
    const n1 = tryParseNumber(v1str);
    const n2 = tryParseNumber(v2str);

    if (operator === '-') {
      if (d1 && d2) {
        const diffMs = d1.getTime() - d2.getTime();
        return String(Math.round(diffMs / (1000 * 60 * 60 * 24)));
      }
      if (d1 && n2 !== null) {
        const result = new Date(d1);
        result.setDate(result.getDate() - n2);
        return formatDate(result);
      }
      if (n1 !== null && n2 !== null) {
        return String(Math.round(n1 - n2));
      }
      return null;
    }

    if (operator === '+') {
      if (d1 && n2 !== null) {
        const result = new Date(d1);
        result.setDate(result.getDate() + n2);
        return formatDate(result);
      }
      if (n1 !== null && n2 !== null) {
        return String(Math.round(n1 + n2));
      }
      return null;
    }

    return null;
  }

  // Compute a chain of operations: values[] with operators[] between them
  // Returns the final result as a string, or null if incompatible
  function computeChain(values, operators) {
    var accumulator = values[0];
    for (var i = 0; i < operators.length; i++) {
      accumulator = computeStep(accumulator, values[i + 1], operators[i]);
      if (accumulator === null) return null;
    }
    return accumulator;
  }

  // Execute the calc command on visible tasks
  function executeCalcOnTasks(command) {
    const parsed = parseCalcCommand(command);
    if (!parsed) {
      syscall('editor.flashNotification', 'Invalid syntax. Expected: attr [+|-] attr|num [+|-] ... -> newAttr');
      return;
    }

    const { terms, operators, resultAttr } = parsed;

    // Collect visible task divs
    const container = document.getElementById('explorerGrid');
    if (!container) return;

    const taskDivs = Array.from(
      container.querySelectorAll('.sb-TaskExplorer-div-task')
    ).filter(function(div) {
      return div.style.display !== 'none';
    });

    if (taskDivs.length === 0) {
      syscall('editor.flashNotification', 'No visible tasks');
      return;
    }

    // Build expression string for log
    var exprParts = [terms[0].value];
    for (var i = 0; i < operators.length; i++) {
      exprParts.push(operators[i]);
      exprParts.push(terms[i + 1].value);
    }
    const exprStr = exprParts.join(' ');

    // Analyze each task
    const eligible = [];
    const skippedMissing = [];
    const skippedIncompatible = [];
    const skippedExists = [];

    taskDivs.forEach(function(div) {
      const taskId = div.getAttribute('data-task-id') || '?';

      // Resolve all term values
      const values = [];
      var missing = false;
      for (var i = 0; i < terms.length; i++) {
        var val;
        if (terms[i].isLiteral) {
          val = terms[i].value;
        } else {
          val = extractAttributeValue(div, terms[i].value);
          if (val === null || val === '') {
            missing = true;
            break;
          }
        }
        values.push(val);
      }

      if (missing) {
        skippedMissing.push(taskId);
        return;
      }

      // Compute chain
      const result = computeChain(values, operators);
      if (result === null) {
        skippedIncompatible.push(taskId);
        return;
      }

      // Constraint: result attribute must not already exist
      const existingVal = extractAttributeValue(div, resultAttr);
      if (existingVal !== null && existingVal !== '') {
        skippedExists.push(taskId);
        return;
      }

      // Build detail string for log: val1 + val2 - val3 = result
      var detailParts = [];
      detailParts.push(values[0]);
      for (var j = 0; j < operators.length; j++) {
        detailParts.push(operators[j]);
        detailParts.push(values[j + 1]);
      }

      eligible.push({
        taskId: taskId,
        detail: detailParts.join(' '),
        result: result
      });
    });

    // Build log for confirmation
    var logLines = [];
    logLines.push('Command: ' + exprStr + ' -> ' + resultAttr);
    logLines.push('');

    if (eligible.length > 0) {
      logLines.push('Will update ' + eligible.length + ' task(s):');
      eligible.forEach(function(item) {
        logLines.push('  ' + item.taskId + ': ' + item.detail + ' = ' + item.result);
      });
    }

    if (skippedMissing.length > 0) {
      logLines.push('');
      logLines.push('Skipped (missing attribute): ' + skippedMissing.length);
      skippedMissing.forEach(function(id) { logLines.push('  ' + id); });
    }
    if (skippedIncompatible.length > 0) {
      logLines.push('');
      logLines.push('Skipped (incompatible values): ' + skippedIncompatible.length);
      skippedIncompatible.forEach(function(id) { logLines.push('  ' + id); });
    }
    if (skippedExists.length > 0) {
      logLines.push('');
      logLines.push('Skipped (' + resultAttr + ' already exists): ' + skippedExists.length);
      skippedExists.forEach(function(id) { logLines.push('  ' + id); });
    }

    if (eligible.length === 0) {
      var summary = 'No eligible tasks.';
      if (skippedMissing.length > 0) summary += ' Missing attr: ' + skippedMissing.length + '.';
      if (skippedIncompatible.length > 0) summary += ' Incompatible types: ' + skippedIncompatible.length + ' (date+date is invalid, use date-date or date+/-number).';
      if (skippedExists.length > 0) summary += ' Already exist: ' + skippedExists.length + '.';
      syscall('editor.flashNotification', summary);
      //console.log(logLines.join('\n'));
      return;
    }

    const logText = logLines.join('\n');
    const confirmMessage = 'Add attribute "' + resultAttr + '" to ' + eligible.length + ' task(s)?';

    // Prepare parallel arrays for Lua
    const calcTaskIds = eligible.map(function(item) { return item.taskId; });
    const calcValues = eligible.map(function(item) { return String(item.result); });

    // Send everything to Lua which handles confirmation modal + writing
    syscall('editor.invokeCommand', 'TaskExplorer: CalcBatchUpdate', [
      calcTaskIds,
      resultAttr,
      calcValues,
      logText,
      confirmMessage
    ]);
  }

  // !!!=========================================
  // 7. DATA RECOVERY — Attributes and ITags
  // ============================================
  function getUniqueAttributes() {

    const dateVersion = sessionStorage.getItem("dateVersion");
    let attributeSpans = "";
    if (dateVersion >= "2026-02-06") {
      attributeSpans = document.querySelectorAll(
      ".sb-list.sb-frontmatter.sb-attribute-name",
      );
    } else {
      attributeSpans = document.querySelectorAll(
        ".sb-list.sb-frontmatter.sb-atom",
      );
    }

    const attributeSet = new Set();

    attributeSpans.forEach(function (span) {
      const attributeName = span.textContent.trim();
      const cleanName = attributeName.replace(/:$/, "");
      if (cleanName) {
        attributeSet.add(cleanName);
      }
    });

    return Array.from(attributeSet).sort(function (a, b) {
      return a.toLowerCase().localeCompare(b.toLowerCase());
    });
  }

  // --- ITags ---
  function getUniqueItags() {
    const itagsSet = new Set();
    const taskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');

    taskDivs.forEach(function(taskDiv) {
      const childDiv = taskDiv.querySelector('#divTaskchild');
      if (!childDiv) return;

      const title = childDiv.getAttribute('title');
      if (!title) return;

      const itagsMatch = title.match(/ITAGS:\s*\{([^}]+)\}/);
      if (!itagsMatch) return;

      const itagsContent = itagsMatch[1];
      const itags = itagsContent.split(',');

      itags.forEach(function(itag) {
        const trimmed = itag.trim();
        if (trimmed && trimmed !== 'task') {
          itagsSet.add(trimmed);
        }
      });
    });

    return Array.from(itagsSet).sort(function(a, b) {
      return a.toLowerCase().localeCompare(b.toLowerCase());
    });
  }

  // --- Attribute Values ---
  function getUniqueValuesForAttribute(attrName) {
    const valuesSet = new Set();
    const taskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');

    taskDivs.forEach(function(taskDiv) {
      const val = extractAttributeValue(taskDiv, attrName);
      if (val && val !== '..' && val.trim() !== '') {
        valuesSet.add(val.trim());
      }
    });

    return Array.from(valuesSet).sort(function(a, b) {
      return a.toLowerCase().localeCompare(b.toLowerCase());
    });
  }

  // !!!=========================================
  // 8. POPUP — Generic popup for Attributes and ITags
  // ============================================

  function showItemsPopup(event, searchInput, slashPosition, type) {

    const config = {
      attributes: {
        getItems: getUniqueAttributes,
        displayPrefix: '',
        insertFn: insertAttributeAtCursor,
        popupId: 'itemsPopup',
        borderColor: '#007bff',
        hintText: 'ATTRIBUTES in the list (ESC to cancel)'
      },
      itags: {
        getItems: getUniqueItags,
        displayPrefix: '#',
        insertFn: insertItagAtCursor,
        popupId: 'itemsPopup',
        borderColor: '#0066cc',
        hintText: 'ITAGS in the list (ESC to cancel)'
      }
    };

    const cfg = config[type];
    const popup = document.getElementById(cfg.popupId);
    const items = cfg.getItems();
    if (items.length === 0) {
      return;
    }

    searchInput._slashPosition = slashPosition !== undefined ? slashPosition : null;
    searchInput._selectedOperator = null;

    // Create or reuse popup
    let popupEl = popup || document.createElement("div");
    if (!popup) {
      popupEl.id = cfg.popupId;
      popupEl.className = "popup-" + type;
      document.body.appendChild(popupEl);
    } else {
      popupEl.className = "popup-" + type;
    }
    popupEl.innerHTML = "";

    // ==================== OPERATORS BAR ====================
    const operators = getOperatorsForMode(searchMode);

    if (operators.length > 0) {
      const operatorBar = document.createElement("div");
      operatorBar.className = "popup-operator-bar";

      operators.forEach(function(op) {
        const opBtn = document.createElement("button");
        opBtn.textContent = op.label;
        opBtn.setAttribute('data-operator', op.value);
        opBtn.className = "popup-operator-btn";

        opBtn.addEventListener('mousedown', function(e) {
          e.preventDefault();
        });

        opBtn.addEventListener('click', function(e) {
          e.stopPropagation();
          e.preventDefault();

          operatorBar.querySelectorAll('button').forEach(function(btn) {
            btn.classList.remove('selected');
          });

          this.classList.add('selected');

          searchInput._selectedOperator = op.value;
          searchInput.focus();
        });

        operatorBar.appendChild(opBtn);
      });

      popupEl.appendChild(operatorBar);
    }

    // ==================== HINT BAR ====================
    const hint = document.createElement("div");
    hint.textContent = cfg.hintText;
    hint.className = "popup-hint";
    popupEl.appendChild(hint);

    // ==================== ITEMS LIST ====================
    const listContainer = document.createElement("div");
    listContainer.className = "popup-list-container";

    items.forEach(function(item, index) {
      const itemDiv = document.createElement("div");
      itemDiv.textContent = cfg.displayPrefix + item;
      itemDiv.setAttribute('data-item', item);
      itemDiv.setAttribute('data-index', index);
      itemDiv.className = "popup-item" + (type === 'itags' ? " popup-item-itag" : "");

      itemDiv.addEventListener("mousedown", function(e) {
        e.preventDefault();
      });

      itemDiv.addEventListener("mouseenter", function() {
        listContainer.querySelectorAll('[data-item]').forEach(function(el) {
          el.classList.remove('popup-item-highlight');
        });
        this.classList.add('popup-item-highlight');
      });

      itemDiv.addEventListener("mouseleave", function() {
        this.classList.remove('popup-item-highlight');
      });

      itemDiv.addEventListener("click", function() {
        searchInput._insertingAttribute = true;
        cfg.insertFn(item, searchInput);
        popupEl.style.display = "none";

        setTimeout(function() {
          searchInput._insertingAttribute = false;
        }, 100);
      });

      listContainer.appendChild(itemDiv);
    });

    popupEl.appendChild(listContainer);

    // ==================== POSITION ====================
    let left = event.clientX + 10;
    let top = event.clientY + 10;

    if (left + 220 > window.innerWidth) {
      left = event.clientX - 230;
    }
    if (top + 350 > window.innerHeight) {
      top = event.clientY - 360;
    }

    popupEl.style.left = left + "px";
    popupEl.style.top = top + "px";
    popupEl.style.display = "block";

    // Keep the focus on the input to allow you to continue typing
    // (ex: // after /).
    // The keyHandler will capture keys for navigation/search

    // ==================== KEYBOARD NAVIGATION ====================
    let selectedIndex = 0;
    let searchBuffer = "";
    let searchTimeout = null;

    const itemElements = listContainer.querySelectorAll('[data-item]');

    function updateSelection(newIndex) {
      itemElements.forEach(function(item) {
        item.classList.remove('popup-item-highlight');
      });

      if (itemElements[newIndex]) {
        selectedIndex = newIndex;
        itemElements[selectedIndex].classList.add('popup-item-highlight');

        itemElements[selectedIndex].scrollIntoView({
          block: 'nearest',
          behavior: 'smooth'
        });
      }
    }

    function searchAndSelect(searchTerm) {
      for (let i = 0; i < itemElements.length; i++) {
        const itemText = itemElements[i].textContent.toLowerCase();
        const searchText = itemText.startsWith('#') || itemText.startsWith(cfg.displayPrefix)
          ? itemText.substring(cfg.displayPrefix.length)
          : itemText;

        if (searchText.startsWith(searchTerm.toLowerCase())) {
          updateSelection(i);
          return true;
        }
      }
      return false;
    }

    updateSelection(0);

    // Remove old keyHandler if it exists
    if (popupEl._keyHandler) {
      document.removeEventListener("keydown", popupEl._keyHandler);
    }

    const keyHandler = function(e) {
      if (popupEl.style.display === "none") {
        document.removeEventListener("keydown", keyHandler);
        return;
      }

      // Pass the "/" to the input to allow "//"
      if (e.key === "/") {
        return; // Do not capture
      }

      if (e.key === "ArrowDown") {
        e.preventDefault();
        updateSelection(Math.min(selectedIndex + 1, itemElements.length - 1));
        searchBuffer = "";
      }
      else if (e.key === "ArrowUp") {
        e.preventDefault();
        updateSelection(Math.max(selectedIndex - 1, 0));
        searchBuffer = "";
      }
      else if (e.key === "Enter") {
        e.preventDefault();
        if (itemElements[selectedIndex]) {
          itemElements[selectedIndex].click();
        }
      }
      else if (e.key === "Escape") {
        e.preventDefault();
        e.stopPropagation();

        searchInput._popupClosing = true;

        popupEl.style.display = "none";
        searchInput._slashPosition = null;
        searchInput._selectedOperator = null;
        searchBuffer = "";
        document.removeEventListener("keydown", keyHandler);
        delete popupEl._keyHandler; // Clean reference
        searchInput.focus();

        setTimeout(function() {
          searchInput._popupClosing = false;
        }, 50);
      }
      else if (e.key.length === 1 && /^[a-zA-Z0-9_-]$/.test(e.key)) {
        // Alphanumeric search in the popup
        e.preventDefault();
        searchBuffer += e.key;
        searchAndSelect(searchBuffer);

        clearTimeout(searchTimeout);
        searchTimeout = setTimeout(function() {
          searchBuffer = "";
        }, 1000);
      }
      else if (e.key === "Backspace" && searchBuffer.length > 0) {
        e.preventDefault();
        searchBuffer = searchBuffer.slice(0, -1);
        if (searchBuffer.length > 0) {
          searchAndSelect(searchBuffer);
        }
      }
    };

    // Save the keyHandler in the popup so you can delete it later
    popupEl._keyHandler = keyHandler;
    document.addEventListener("keydown", keyHandler);

    // ==================== CLOSE ON CLICK OUTSIDE ====================
    setTimeout(function() {
      const closeHandler = function(e) {
        if (!popupEl.contains(e.target) && e.target !== searchInput) {
          popupEl.style.display = "none";
          searchInput._slashPosition = null;
          searchInput._selectedOperator = null;
          document.removeEventListener("click", closeHandler);
          document.removeEventListener("keydown", keyHandler);
          delete popupEl._keyHandler; // Nettoyer la référence
        }
      };
      document.addEventListener("click", closeHandler);
    }, 100);
  }

  // !!!=========================================
  // 8b. POPUP — Values popup for a specific attribute
  // ============================================

  function detectAttributeContext(value, cursorPos) {
    var slashPos = cursorPos - 1;
    var textBeforeSlash = value.substring(0, slashPos);
    var openBracket = String.fromCharCode(91);
    var closeBracket = String.fromCharCode(93);

    if (searchMode === "list") {
      var lastOpen = textBeforeSlash.lastIndexOf(openBracket);
      if (lastOpen === -1) return null;

      var segment = textBeforeSlash.substring(lastOpen + 1);
      if (segment.indexOf(closeBracket) !== -1) return null;

      var colonIdx = segment.indexOf(':');
      if (colonIdx === -1) return null;

      var attrName = segment.substring(0, colonIdx).trim();
      if (attrName.length === 0) return null;

      var afterColon = segment.substring(colonIdx + 1);
      var hasOnlySpaces = (afterColon.trim().length === 0);
      if (!hasOnlySpaces) return null;

      return { attrName: attrName, slashPos: slashPos };
    }

    if (searchMode === "where") {
      var eqPos = textBeforeSlash.lastIndexOf("=='");
      if (eqPos === -1) {
        eqPos = textBeforeSlash.lastIndexOf('=="');
      }
      if (eqPos === -1) return null;

      var quoteEnd = eqPos + 3;
      if (quoteEnd !== textBeforeSlash.length) return null;

      var beforeEq = textBeforeSlash.substring(0, eqPos);
      var dotPos = beforeEq.lastIndexOf('_.');
      if (dotPos === -1) return null;

      var attrName = beforeEq.substring(dotPos + 2);
      if (attrName.length === 0) return null;
      if (!(/^[a-zA-Z0-9_-]+$/).test(attrName)) return null;

      return { attrName: attrName, slashPos: slashPos };
    }

    return null;
  }

  function showValuesPopup(event, searchInput, context) {
    const attrName = context.attrName;
    const items = getUniqueValuesForAttribute(attrName);
    if (items.length === 0) return;

    const popupId = 'itemsPopup';
    const popup = document.getElementById(popupId);

    // Create or reuse popup — reuse popup-attributes class for consistent styling
    let popupEl = popup || document.createElement("div");
    if (!popup) {
      popupEl.id = popupId;
      popupEl.className = "popup-attributes";
      document.body.appendChild(popupEl);
    } else {
      popupEl.className = "popup-attributes";
    }
    popupEl.innerHTML = "";

    // ===== OPERATOR BAR (empty, for visual consistency) =====
    const operatorBar = document.createElement("div");
    operatorBar.className = "popup-operator-bar";
    popupEl.appendChild(operatorBar);

    // ==================== HINT BAR ====================
    const hint = document.createElement("div");
    var closeBracket = String.fromCharCode(93);
    hint.textContent = 'VALUES of ' + String.fromCharCode(91) + attrName + closeBracket + ' (ESC to cancel)';
    hint.className = "popup-hint";
    popupEl.appendChild(hint);

    // ==================== ITEMS LIST ====================
    const listContainer = document.createElement("div");
    listContainer.className = "popup-list-container";

    items.forEach(function(item, index) {
      const itemDiv = document.createElement("div");
      itemDiv.textContent = item;
      itemDiv.setAttribute('data-item', item);
      itemDiv.setAttribute('data-index', index);
      itemDiv.className = "popup-item";

      itemDiv.addEventListener("mousedown", function(e) {
        e.preventDefault();
      });

      itemDiv.addEventListener("mouseenter", function() {
        listContainer.querySelectorAll('[data-item]').forEach(function(el) {
          el.classList.remove('popup-item-highlight');
        });
        this.classList.add('popup-item-highlight');
      });

      itemDiv.addEventListener("mouseleave", function() {
        this.classList.remove('popup-item-highlight');
      });

      itemDiv.addEventListener("click", function() {
        searchInput._insertingAttribute = true;
        insertValueAtCursor(item, searchInput, context);
        popupEl.style.display = "none";

        setTimeout(function() {
          searchInput._insertingAttribute = false;
        }, 100);
      });

      listContainer.appendChild(itemDiv);
    });

    popupEl.appendChild(listContainer);

    // ==================== POSITION ====================
    let left = event.clientX + 10;
    let top = event.clientY + 10;

    if (left + 220 > window.innerWidth) {
      left = event.clientX - 230;
    }
    if (top + 350 > window.innerHeight) {
      top = event.clientY - 360;
    }

    popupEl.style.left = left + "px";
    popupEl.style.top = top + "px";
    popupEl.style.display = "block";
    let selectedIndex = 0;
    let searchBuffer = "";
    let searchTimeout = null;

    const itemElements = listContainer.querySelectorAll('[data-item]');

    function updateSelection(newIndex) {
      itemElements.forEach(function(item) {
        item.classList.remove('popup-item-highlight');
      });

      if (itemElements[newIndex]) {
        selectedIndex = newIndex;
        itemElements[selectedIndex].classList.add('popup-item-highlight');

        itemElements[selectedIndex].scrollIntoView({
          block: 'nearest',
          behavior: 'smooth'
        });
      }
    }

    function searchAndSelect(searchTerm) {
      for (let i = 0; i < itemElements.length; i++) {
        const itemText = itemElements[i].textContent.toLowerCase();
        if (itemText.startsWith(searchTerm.toLowerCase())) {
          updateSelection(i);
          return true;
        }
      }
      return false;
    }

    updateSelection(0);

    // Remove old keyHandler if it exists
    if (popupEl._keyHandler) {
      document.removeEventListener("keydown", popupEl._keyHandler);
    }

    const keyHandler = function(e) {
      if (popupEl.style.display === "none") {
        document.removeEventListener("keydown", keyHandler);
        return;
      }

      if (e.key === "ArrowDown") {
        e.preventDefault();
        updateSelection(Math.min(selectedIndex + 1, itemElements.length - 1));
        searchBuffer = "";
      }
      else if (e.key === "ArrowUp") {
        e.preventDefault();
        updateSelection(Math.max(selectedIndex - 1, 0));
        searchBuffer = "";
      }
      else if (e.key === "Enter") {
        e.preventDefault();
        if (itemElements[selectedIndex]) {
          itemElements[selectedIndex].click();
        }
      }
      else if (e.key === "Escape") {
        e.preventDefault();
        e.stopPropagation();

        searchInput._popupClosing = true;

        popupEl.style.display = "none";
        searchBuffer = "";
        document.removeEventListener("keydown", keyHandler);
        delete popupEl._keyHandler;
        searchInput.focus();

        setTimeout(function() {
          searchInput._popupClosing = false;
        }, 50);
      }
      else if (e.key.length === 1 && /^[a-zA-Z0-9_\-. ]$/.test(e.key)) {
        // Alphanumeric search in the popup (allow spaces/dots/dashes for value search)
        e.preventDefault();
        searchBuffer += e.key;
        searchAndSelect(searchBuffer);

        clearTimeout(searchTimeout);
        searchTimeout = setTimeout(function() {
          searchBuffer = "";
        }, 1000);
      }
      else if (e.key === "Backspace" && searchBuffer.length > 0) {
        e.preventDefault();
        searchBuffer = searchBuffer.slice(0, -1);
        if (searchBuffer.length > 0) {
          searchAndSelect(searchBuffer);
        }
      }
    };

    popupEl._keyHandler = keyHandler;
    document.addEventListener("keydown", keyHandler);

    // ==================== CLOSE ON CLICK OUTSIDE ====================
    setTimeout(function() {
      const closeHandler = function(e) {
        if (!popupEl.contains(e.target) && e.target !== searchInput) {
          popupEl.style.display = "none";
          document.removeEventListener("click", closeHandler);
          document.removeEventListener("keydown", keyHandler);
          delete popupEl._keyHandler;
        }
      };
      document.addEventListener("click", closeHandler);
    }, 100);
  }

  function insertValueAtCursor(val, searchInput, context) {
    const currentValue = searchInput.value;
    const slashPos = context.slashPos;

    // Remove the "/" that triggered the popup
    const valueWithoutSlash = currentValue.substring(0, slashPos) +
                              currentValue.substring(slashPos + 1);

    // Insert the value at the slash position
    const newValue = valueWithoutSlash.substring(0, slashPos) +
                     val +
                     valueWithoutSlash.substring(slashPos);

    searchInput.value = newValue;

    // Position cursor after the inserted value
    const newCursorPos = slashPos + val.length;
    searchInput.setSelectionRange(newCursorPos, newCursorPos);

    // Trigger appropriate action if in list mode
    if (searchMode === "list") {
      sessionStorage.setItem("searchTermFilter", newValue);
      applySearchFilter(newValue);
    }

    searchInput.focus();
  }

  // !!!=========================================
  // 9. CONTEXTUAL MENU — Create, Show, Hide
  // ============================================

  function createAttributeMenu() {
    const existingMenu = document.getElementById("attributeDropdownMenu");
    if (existingMenu) {
      existingMenu.remove();
    }

    const menu = document.createElement("div");
    menu.id = "attributeDropdownMenu";
    document.body.appendChild(menu);
    return menu;
  }

  function createSubMenu() {
    const existingSubMenu = document.getElementById("attributeSubMenu");
    if (existingSubMenu) {
      existingSubMenu.remove();
    }

    const subMenu = document.createElement("div");
    subMenu.id = "attributeSubMenu";
    document.body.appendChild(subMenu);
    return subMenu;
  }

  function hideSubMenu() {
    const subMenu = document.getElementById("attributeSubMenu");
    if (subMenu) {
      subMenu.style.display = "none";
    }
  }

  function hideAttributeMenu() {
    const menu = document.getElementById("attributeDropdownMenu");
    if (menu) {
      menu.style.display = "none";
    }
    hideSubMenu();
  }

  function showSubMenu(parentItem, items, type, searchInput) {
    const subMenu =
      document.getElementById("attributeSubMenu") || createSubMenu();
    subMenu.innerHTML = "";

    items.forEach(function (item) {
      const itemDiv = document.createElement("div");

      if (type === "example" && typeof item === 'object' && item.clause) {
        itemDiv.innerHTML = `
          <div class="submenu-example-clause">${item.clause}</div>
          <div class="submenu-example-comment">${item.comment}</div>
        `;
      } else {
        itemDiv.textContent = item;
      }

      itemDiv.className = "submenu-item";

      itemDiv.addEventListener("click", function (e) {
        e.stopPropagation();

        const currentValue = searchInput.value;

        if (type === "property") {
          let prefix = "";
          if (currentValue) {
            prefix = " and _.";
          } else {
            prefix = "_.";
          }
          searchInput.value = currentValue + prefix + item + "==' '";
        } else if (type === "example") {
          const clauseText = typeof item === 'object' ? item.clause : item;
          searchInput.value = currentValue + clauseText;
        } else if (type === "history") {
          searchInput.value = currentValue + item.substring(5);
        } else if (type === "attribute") {
          let prefix = "";
          if (searchMode === "list") {
            prefix = currentValue && !currentValue.endsWith(" ") ? " [" : "[";
            searchInput.value = currentValue + prefix + item + ": ]";
          } else {
            if (currentValue) {
              prefix = " and _.";
            } else {
              prefix = "_.";
            }
            searchInput.value = currentValue + prefix + item + "==' '";
          }
          sessionStorage.setItem("searchTermFilter", searchInput.value);
          if (searchMode === "list") {
            applySearchFilter(searchInput.value);
          }
        } else if (type === "attribute_update") {
          searchInput.value = currentValue + item;
          } else if (type === "attribute_sort") {
            const prefix = currentValue && !currentValue.endsWith(" ") ? " " : "";
            searchInput.value = currentValue + prefix + item + "+";
          } else if (type === "attribute_calc") {
            const currentVal = currentValue.trim();
            const hasArrow = currentVal.indexOf('->') !== -1;
            if (hasArrow) {
              searchInput.value = currentVal + ' ' + item;
            } else {
              const prefix = currentVal ? ' ' : '';
              searchInput.value = currentVal + prefix + item;
            }
          } else if (type === "attribute_stat") {
            const prefix = currentValue && !currentValue.endsWith(" ") ? " " : "";
            searchInput.value = currentValue + prefix + item;
          }

        searchInput.focus();
        hideSubMenu();
      });

      subMenu.appendChild(itemDiv);
    });

    const rect = parentItem.getBoundingClientRect();
    subMenu.style.left = rect.right + 5 + "px";
    subMenu.style.top = rect.top + "px";
    subMenu.style.display = "block";
  }

  // !!!=========================================
  // 10. MODE MENU — Mode buttons bar and menu content
  // ============================================

  const MENU_MODES = [
    { id: "listMode", label: "Filter", mode: "list", activeClass: "active-filter" },
    { id: "sortMode", label: "Sort", mode: "sort", activeClass: "active-sort" },
    { id: "calcMode", label: "Calc", mode: "calc", activeClass: "active-calc" },
    { id: "updateMode", label: "Update", mode: "update", activeClass: "active-update" },
    { id: "statMode", label: "Stat", mode: "stat", activeClass: "active-stat" },
    { id: "whereMode",  label: "Query",  mode: "where",  activeClass: "active-query" }
  ];

  function showMenuForCurrentMode(searchInput, hideMenu) {
    const menu =
      document.getElementById("attributeDropdownMenu") || createAttributeMenu();
    menu.innerHTML = "";

    // ---- Mode buttons bar ----
    const modeTitle = document.createElement("div");
    modeTitle.className = "menu-mode-title mode-switcher";

    MENU_MODES.forEach(function(m) {
      const btn = document.createElement("button");
      btn.id = m.id;
      btn.textContent = m.label;
      btn.title = m.label + " mode";

      const isActive = (m.mode === searchMode);
      btn.className = "menu-mode-btn" + (isActive ? " " + m.activeClass : "") + " mode-switcher";

      btn.onclick = function(e) {
        e.stopPropagation();
        window.switchSearchMode(m.mode, searchInput);
      };

      modeTitle.appendChild(btn);
    });

    // ---- Info button (💡 emoji) ----
    const infoBtn = document.createElement("span");
    infoBtn.className = "mode-switcher";
    infoBtn.title = "Instructions for use";
    infoBtn.style.cssText = "margin-left: auto; margin-right: 4px; cursor: pointer; font-size: 18px; line-height: 1; padding: 0 2px;";
    infoBtn.textContent = "\uD83D\uDCA1";
    infoBtn.addEventListener("mousedown", function(e) {
      e.preventDefault();
      e.stopPropagation();
    });
    infoBtn.addEventListener("click", function(e) {
      e.stopPropagation();
      syscall('editor.invokeCommand', 'Open: Instructions');
    });
    modeTitle.appendChild(infoBtn);

    menu.appendChild(modeTitle);

    // ---- Additional items for Where mode: horizontal tab bar ----
    if (searchMode === "where") {
      const tabBar = document.createElement("div");
      tabBar.className = "query-tab-bar";

      // Container for dropdown content (inserted below tab bar in the menu)
      const dropZone = document.createElement("div");
      dropZone.className = "query-drop-zone";
      dropZone.style.display = "none";

      var activeTabLabel = null;

      var tabDefs = [
        { label: "Properties", getItems: function() { return properties; }, emptyMsg: "No properties", type: "property" },
        { label: "History", getItems: function() { return history; }, emptyMsg: "No history yet", type: "history" },
        { label: "Examples", getItems: function() { return examples; }, emptyMsg: "No examples available", type: "example" }
      ];

      tabDefs.forEach(function(def) {
        var tabSpan = document.createElement("span");
        tabSpan.className = "query-tab-btn";
        tabSpan.textContent = def.label + " \u25BE";

        tabSpan.addEventListener("mousedown", function(e) {
          e.preventDefault();
          e.stopPropagation();
        });

        tabSpan.addEventListener("mouseenter", function(e) {
          e.stopPropagation();

          activeTabLabel = def.label;
          tabBar.querySelectorAll(".query-tab-btn").forEach(function(b) { b.classList.remove("query-tab-active"); });
          tabSpan.classList.add("query-tab-active");

          // Fill drop zone
          dropZone.innerHTML = "";
          var items = def.getItems();

          if (!items || items.length === 0) {
            var emptyDiv = document.createElement("div");
            emptyDiv.className = "submenu-item query-drop-item";
            emptyDiv.style.color = "#999";
            emptyDiv.textContent = def.emptyMsg;
            dropZone.appendChild(emptyDiv);
          } else {
            items.forEach(function(item) {
              var row = document.createElement("div");
              row.className = "submenu-item query-drop-item";

              if (def.type === "example" && typeof item === 'object' && item.clause) {
                row.innerHTML =
                  '<div class="submenu-example-clause">' + item.clause + '</div>' +
                  '<div class="submenu-example-comment">' + item.comment + '</div>';
              } else {
                row.textContent = item;
              }

              row.addEventListener("click", function(ev) {
                ev.stopPropagation();
                var cv = searchInput.value;
                if (def.type === "property") {
                  var pfx = cv ? " and _." : "_.";
                  searchInput.value = cv + pfx + item + "==' '";
                } else if (def.type === "example") {
                  searchInput.value = cv + (typeof item === 'object' ? item.clause : item);
                } else if (def.type === "history") {
                  searchInput.value = cv + item.substring(5);
                }
                searchInput.focus();
                updateClearBtnVisibility(searchInput.value);
                dropZone.style.display = "none";
                tabBar.querySelectorAll(".query-tab-btn").forEach(function(b) { b.classList.remove("query-tab-active"); });
                activeTabLabel = null;
              });

              dropZone.appendChild(row);
            });
          }
          dropZone.style.display = "block";
        });

        tabBar.appendChild(tabSpan);
      });

      menu.appendChild(tabBar);
      menu.appendChild(dropZone);

      // Hide dropZone when mouse leaves the entire menu area
      menu.addEventListener("mouseleave", function() {
        dropZone.style.display = "none";
        tabBar.querySelectorAll(".query-tab-btn").forEach(function(b) { b.classList.remove("query-tab-active"); });
        activeTabLabel = null;
      });
    }

    const rect = searchInput.getBoundingClientRect();
    menu.style.left = rect.left + "px";
    menu.style.top = rect.bottom + 2 + "px";
    menu.style.width = (searchMode === "sort" ? rect.width / 2 : rect.width * 0.65) + "px";

    // In where mode, the dropZone handles its own scroll;
    // the parent menu must not clip or double-scroll.
    if (searchMode === "where") {
      menu.style.maxHeight = "none";
      menu.style.overflowY = "visible";
    } else {
      menu.style.maxHeight = "";
      menu.style.overflowY = "";
    }

    if (hideMenu === "true") {
      menu.style.display = "none";
    } else {
      menu.style.display = "block";
    }
  }

  // !!!=========================================
  // 11. MODE SWITCHING
  // ============================================

  window.switchSearchMode = function(mode, searchInput) {
    hideSubMenu();
    searchMode = mode;

    const inputSearchZone = document.querySelector('#tileSearch');
    document.querySelectorAll('.mode-switcher div').forEach(el => el.classList.remove('active'));

    if (mode === "list") {
      sessionStorage.setItem("searchMode", "list");
      const btn = document.querySelector('#listMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 real-time filter (Esc to exit)";
      inputSearchZone.className = "mode-list";
      // Restore filter text if a filter is active
      const activeFilter = sessionStorage.getItem('searchTermFilter') || '';
      searchInput.value = activeFilter;
    } else if (mode === "where") {
      searchInput.value = "";
      sessionStorage.setItem("searchMode", "where");
      const btn = document.querySelector('#whereMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 custom clause where (Enter to query)";
      inputSearchZone.className = "mode-where";
    } else if (mode === "update") {
      searchInput.value = "";
      sessionStorage.setItem("searchMode", "update");
      const btn = document.querySelector('#updateMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 add:name[=value] | delete:name | rename:old->new (Enter to update)";
      inputSearchZone.className = "mode-update";
    } else if (mode === "sort") {
      sessionStorage.setItem("searchMode", "sort");
      const btn = document.querySelector('#sortMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 attr1[+|-][ attr2+|-][ !] to keep all (Enter to sort)";
      inputSearchZone.className = "mode-sort";
      // Restore sort criteria if a sort is active
      const container = document.getElementById('explorerGrid');
      const activeSortCriteria = container ? container.getAttribute('data-sort-active') || '' : '';
      searchInput.value = activeSortCriteria;
    } else if (mode === "calc") {
      searchInput.value = "";
      sessionStorage.setItem("searchMode", "calc");
      const btn = document.querySelector('#calcMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 a [+|-] b [+|-] c... -> result (Enter to calculate)";
      inputSearchZone.className = "mode-calc";
    } else if (mode === "stat") {
      searchInput.value = "";
      sessionStorage.setItem("searchMode", "stat");
      const btn = document.querySelector('#statMode');
      btn.classList.add('active');
      inputSearchZone.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 attr1 attr2... (Enter to compute stats)";
      inputSearchZone.className = "mode-stat";
    }

    updateClearBtnVisibility(searchInput.value);
    searchInput.focus();
    showMenuForCurrentMode(searchInput, "false");
  }

  // !!!=========================================
  // 12. EVENT HANDLERS — Input
  // ============================================

  // --- Focus: show mode menu ---
  function handleSearchFocus(searchInput) {
    if (searchInput._insertingAttribute) return;
    setTimeout(function() {
      showMenuForCurrentMode(searchInput, "false");
    }, 50);
  }

  // --- Blur: hide menu if focus left input and menu area ---
  function handleSearchBlur(searchInput, e) {
    const menu = document.getElementById("attributeDropdownMenu");
    const subMenu = document.getElementById("attributeSubMenu");
    const relatedTarget = e.relatedTarget;
    if (relatedTarget &&
        ((menu && menu.contains(relatedTarget)) ||
         (subMenu && subMenu.contains(relatedTarget)))) {
      return;
    }
    setTimeout(function() {
      hideAttributeMenu();
    }, 200);
  }

  // --- Context menu: show mode menu on right-click ---
  function handleSearchContextMenu(searchInput, e) {
    e.preventDefault();
    setTimeout(function() {
      showMenuForCurrentMode(searchInput, "false");
    }, 50);
  }

  // --- Input: react to typing (slash popups, filter, clear button) ---
  function handleSearchInput(searchInput, e) {
    const value = searchInput.value;
    const cursorPos = searchInput.selectionStart;
    const existingPopup = document.getElementById("itemsPopup");

    updateClearBtnVisibility(value);

    // Check for triple slash "///" -> insert taskId double brackets (list mode only)
    var dblBracketOpen = String.fromCharCode(91, 91);
    var dblBracketClose = String.fromCharCode(93, 93);
    if (searchMode === "list" && cursorPos >= 3 &&
        value[cursorPos - 1] === '/' &&
        value[cursorPos - 2] === '/' &&
        value[cursorPos - 3] === '/') {
      if (existingPopup) {
        existingPopup.style.display = "none";
      }
      // Replace the 3 slashes with double brackets and position cursor inside
      var before = value.substring(0, cursorPos - 3);
      var after = value.substring(cursorPos);
      searchInput.value = before + dblBracketOpen + dblBracketClose + after;
      var newPos = before.length + 2; // cursor after opening brackets
      searchInput.setSelectionRange(newPos, newPos);
      return;
    }

    // Check for double slash "//" -> ITAGS popup
    if ((searchMode === "list" || searchMode === "where") && cursorPos >= 2 &&
        value[cursorPos - 1] === '/' &&
        value[cursorPos - 2] === '/') {
      if (existingPopup) {
        existingPopup.style.display = "none";
      }
      const rect = searchInput.getBoundingClientRect();
      const cursorX = rect.left + (cursorPos * 8);
      const cursorY = rect.bottom + 5;
      showItemsPopup({
        clientX: cursorX,
        clientY: cursorY
      }, searchInput, cursorPos - 2, 'itags');
    }
    // Check for single slash "/" -> detect context: VALUE popup or ATTRIBUTES popup
    else if (cursorPos >= 1 &&
             value[cursorPos - 1] === '/' &&
             (cursorPos < 2 || value[cursorPos - 2] !== '/')) {

      // First, check if we are inside an attribute value context (filter or query mode)
      const valContext = (searchMode === "list" || searchMode === "where")
                         ? detectAttributeContext(value, cursorPos)
                         : null;

      if (valContext) {
        // We are inside [attr: /] or _.attr=='/ -> show VALUES popup
        if (existingPopup) {
          existingPopup.style.display = "none";
        }
        const rect = searchInput.getBoundingClientRect();
        const cursorX = rect.left + (cursorPos * 8);
        const cursorY = rect.bottom + 5;
        showValuesPopup({
          clientX: cursorX,
          clientY: cursorY
        }, searchInput, valContext);
      } else {
        // Standard: show ATTRIBUTES popup
        const rect = searchInput.getBoundingClientRect();
        const cursorX = rect.left + (cursorPos * 8);
        const cursorY = rect.bottom + 5;
        showItemsPopup({
          clientX: cursorX,
          clientY: cursorY
        }, searchInput, cursorPos - 1, 'attributes');
      }
    }
    else {
      if (existingPopup && value[cursorPos - 1] !== '/') {
        existingPopup.style.display = "none";
      }
    }

    if (searchMode === "list") {
      applySearchFilter(searchInput.value);
      sessionStorage.setItem("searchTermFilter", searchInput.value);
      const btn = document.querySelector('#listMode');
      btn.title = "Filter : " + searchInput.value
    } else {
      sessionStorage.setItem("searchFilter", searchInput.value);
    }
  }

  // --- Keydown: execute action on Enter (per mode) ---
  function handleSearchKeydown(searchInput, e) {
    if (e.key === "Enter") {
      // If the items popup is open, let its own keyHandler handle Enter
      // (the popup handler on document fires after this one due to bubbling)
      const itemsPopup = document.getElementById("itemsPopup");
      if (itemsPopup && itemsPopup.style.display !== "none") {
        return;
      }
      e.preventDefault();
      hideAttributeMenu();
      if (searchMode === "where") {
        const newWhere = searchInput.value;
        sessionStorage.setItem("searchTerm", searchInput.value);
        sessionStorage.setItem("searchMode", "where");
        searchInput.value = ""
        sessionStorage.setItem("searchTermFilter", "");
        const btn = document.querySelector('#listMode');
        btn.title = "Filter mode : ";
        syscall("editor.invokeCommand", "TaskExplorer: ExecuteQuery", [
          "maj",
          newWhere,
        ]);
      } else if (searchMode === "list") {
        sessionStorage.setItem("searchTermFilter", searchInput.value);
        const btn = document.querySelector('#listMode');
        btn.title = "Filter mode : " + searchInput.value;
        applySearchFilter(searchInput.value);
      } else if (searchMode === "update") {
        const command = searchInput.value.trim();
        if (command) {
          executeAttributeUpdate(command);
        }
      } else if (searchMode === "sort") {
        const sortCriteria = searchInput.value.trim();
        if (sortCriteria) {
          applySortToTasks(sortCriteria);
        }
      } else if (searchMode === "calc") {
        const calcCommand = searchInput.value.trim();
        if (calcCommand) {
          executeCalcOnTasks(calcCommand);
        }
      } else if (searchMode === "stat") {
        const statCommand = searchInput.value.trim();
        if (statCommand) {
          executeStatOnTasks(statCommand);
        }
      }
    }
  }

  // --- Paste / Change: sync filter in list mode ---
  function handleSearchPasteOrChange(searchInput) {
    if (searchMode === "list") {
      applySearchFilter(searchInput.value);
      sessionStorage.setItem("searchTermFilter", searchInput.value);
      const btn = document.querySelector('#listMode');
      btn.title = "Filter : " + searchInput.value
    } else {
      sessionStorage.setItem("searchFilter", searchInput.value);
    }
  }

  // !!!=========================================
  // 12b. EVENT HANDLERS — Document-level
  // ============================================

  // --- Mousedown: prevent menu clicks from stealing focus ---
  function handleDocumentMousedown(searchInput, e) {
    const menu = document.getElementById("attributeDropdownMenu");
    const subMenu = document.getElementById("attributeSubMenu");
    if (menu && menu.contains(e.target)) {
      e.preventDefault();
      searchInput.focus();
    }
    if (subMenu && subMenu.contains(e.target)) {
      e.preventDefault();
    }
  }

  // --- Click: close menu when clicking outside ---
  function handleDocumentClick(searchInput, e) {
    const menu = document.getElementById("attributeDropdownMenu");
    const subMenu = document.getElementById("attributeSubMenu");
    if (menu && !menu.contains(e.target) &&
        subMenu && !subMenu.contains(e.target) &&
        e.target !== searchInput) {
      hideAttributeMenu();
    }
  }

  // --- Escape: progressive dismissal (popup > menu > blur > clear) ---
  function handleDocumentEscape(e) {
    if (e.key !== "Escape") return;

    const searchInput = document.getElementById("tileSearch");
    if (!searchInput) return;

    if (searchInput._popupClosing) return;

    const menu = document.getElementById("attributeDropdownMenu");
    const itemsPopup = document.getElementById("itemsPopup");

    const menuVisible = menu && menu.style.display !== "none";
    const popupVisible = itemsPopup && itemsPopup.style.display !== "none";

    // If the popup is visible, let its handler take care of it
    if (popupVisible) return;

    const inputHasFocus = document.activeElement === searchInput;
    const inputHasContent = searchInput.value.trim() !== "";

    if (menuVisible) {
      e.preventDefault();
      hideAttributeMenu();
      return;
    }

    // Priority 2: Remove focus if input has focus
    if (inputHasFocus) {
      e.preventDefault();
      searchInput.blur();
      return;
    }

    // Priority 3: Clear input if it has content (and doesn't have focus)
    if (inputHasContent && !inputHasFocus) {
      e.preventDefault();
      searchInput.value = "";
      updateClearBtnVisibility('');
      sessionStorage.setItem("searchTermFilter", "");

      if (searchMode === "list") {
        applySearchFilter("");
        const btn = document.querySelector('#listMode');
        if (btn) {
          btn.title = "Filter mode";
        }
      }
      return;
    }
  }

  // !!!=========================================
  // 13. BIND ALL EVENT LISTENERS
  // ============================================

  function initializeSearchMenu(searchInput) {
    searchInput.addEventListener('focus', function(e) { handleSearchFocus(searchInput); });
    searchInput.addEventListener('blur', function(e) { handleSearchBlur(searchInput, e); });
    searchInput.addEventListener('contextmenu', function(e) { handleSearchContextMenu(searchInput, e); });
    searchInput.addEventListener('input', function(e) { handleSearchInput(searchInput, e); });
    searchInput.addEventListener('keydown', function(e) { handleSearchKeydown(searchInput, e); });
    searchInput.addEventListener('paste', function() { handleSearchPasteOrChange(searchInput); });
    searchInput.addEventListener('change', function() { handleSearchPasteOrChange(searchInput); });

    document.addEventListener('mousedown', function(e) { handleDocumentMousedown(searchInput, e); });
    document.addEventListener('click', function(e) { handleDocumentClick(searchInput, e); });
    document.addEventListener('keydown', handleDocumentEscape);
  }

  // !!!=========================================
  // 14. INSERT AT CURSOR — Attributes and ITags
  // ============================================

  function insertAttributeAtCursor(attr, searchInput) {
    const currentValue = searchInput.value;
    let cursorPos = searchInput.selectionStart;
    const selectedOperator = searchInput._selectedOperator;

    // Remove the "/" if it triggered the popup
    let valueBeforeCursor = currentValue;
    if (searchInput._slashPosition !== null && searchInput._slashPosition !== undefined) {
      valueBeforeCursor = currentValue.substring(0, searchInput._slashPosition) +
                         currentValue.substring(searchInput._slashPosition + 1);
      cursorPos = searchInput._slashPosition;
    }

    let insertText = "";

    // ==================== MODE: FILTER ====================
    if (searchMode === "list") {
      if (selectedOperator) {
        // Operator selected: insert operator + [attr: ]
        // Format: operator[attr: ] (no spaces)
        insertText = selectedOperator + '[' + attr + ': ]';
      } else {
        // No operator: just [attr: ]
        const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
        insertText = (needsSpace ? ' ' : '') + '[' + attr + ': ]';
      }
    }

    // ==================== MODE: QUERY ====================
    else if (searchMode === "where") {
      if (selectedOperator) {
        // Operator selected: " operator _.attr==''"
        // Format: space + operator + space + _.attr==''
        insertText = ' ' + selectedOperator + ' _.' + attr + "==''";
      } else {
        // No operator: determine if "and" is needed
        const trimmed = valueBeforeCursor.substring(0, cursorPos).trim();
        const needsAnd = trimmed !== '' && !trimmed.endsWith('and') && !trimmed.endsWith('or');
        insertText = (needsAnd ? ' and _.' : '_.') + attr + "==''";
      }
    }

    // ==================== MODE: UPDATE ====================
    else if (searchMode === "update") {
      if (selectedOperator) {
        // Operator selected: " operator attr" (or "rename:attr->")
        const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
        const prefix = (needsSpace ? ' ' : '') + selectedOperator;

        if (selectedOperator === "rename:") {
          // Special case: rename:attr->
          insertText = prefix + attr + '->';
        } else {
          // add: or delete:
          insertText = prefix + attr;
        }
      } else {
        // No operator: just attr
        insertText = attr;
      }
    }

    // ==================== MODE: SORT ====================
    else if (searchMode === "sort") {
      if (selectedOperator) {
        // Direction operator (+ or -): attr + operator + space
        insertText = attr + selectedOperator + ' ';
      } else {
        // No operator: attr+ (default ascending)
        const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
        insertText = (needsSpace ? ' ' : '') + attr + '+';
      }
    }

    // ==================== MODE: CALC ====================
    else if (searchMode === "calc") {
      const currentVal = valueBeforeCursor.substring(0, cursorPos);
      const hasArrow = currentVal.indexOf('->') !== -1;
      // Has at least one term already (non-empty before any operator or arrow)
      const hasContent = currentVal.trim().length > 0;

      if (hasArrow) {
        // After "->": this is the result attribute name
        insertText = ' ' + attr;
      } else if (selectedOperator === '+' || selectedOperator === '-') {
        // Operator selected: insert " op attr"
        insertText = ' ' + selectedOperator + ' ' + attr;
      } else if (selectedOperator === '->') {
        // Arrow selected: insert " -> attr"
        insertText = ' -> ' + attr;
      } else if (hasContent) {
        // Another term without operator: just insert with space
        insertText = ' ' + attr;
      } else {
        // First attribute
        insertText = attr;
      }
    }

    // ==================== MODE: STAT ====================
    else if (searchMode === "stat") {
      const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
      insertText = (needsSpace ? ' ' : '') + attr;
    }

    // Build new value
    const newValue = valueBeforeCursor.substring(0, cursorPos) +
                     insertText +
                     valueBeforeCursor.substring(cursorPos);

    searchInput.value = newValue;

    // Move cursor after insertion
    let newCursorPos = cursorPos + insertText.length;

    // Special adjustments for cursor position
    if (searchMode === "list") {
      // Position cursor inside [ ]
      newCursorPos = cursorPos + insertText.indexOf(': ]') + 2;
    } else if (searchMode === "where") {
      // Position cursor inside ''
      newCursorPos = cursorPos + insertText.indexOf("''") + 1;
    } else if (searchMode === "update" && selectedOperator === "rename:") {
      // Position cursor after ->
    }

    searchInput.setSelectionRange(newCursorPos, newCursorPos);

    // Clear state
    searchInput._slashPosition = null;
    searchInput._selectedOperator = null;

    // Trigger appropriate action if in list mode
    if (searchMode === "list") {
      sessionStorage.setItem("searchTermFilter", newValue);
      applySearchFilter(newValue);
    }

    searchInput.focus();
  }

  // --- Insert ITag at cursor ---

  function insertItagAtCursor(itag, searchInput) {
    const currentValue = searchInput.value;
    let cursorPos = searchInput.selectionStart;
    const selectedOperator = searchInput._selectedOperator;

    // Remove the "/" if it triggered the popup
    let valueBeforeCursor = currentValue;
    if (searchInput._slashPosition !== null && searchInput._slashPosition !== undefined) {
      valueBeforeCursor = currentValue.substring(0, searchInput._slashPosition) +
                         currentValue.substring(searchInput._slashPosition + 2);
      cursorPos = searchInput._slashPosition;
    }

    let insertText = "";

    // ==================== MODE: FILTER ====================
    if (searchMode === "list") {
      // Detect if cursor is already inside a { ... } block
      const textBeforeCursor = valueBeforeCursor.substring(0, cursorPos);
      const lastOpenBrace = textBeforeCursor.lastIndexOf('{');
      const lastCloseBrace = textBeforeCursor.lastIndexOf('}');
      const insideBraces = lastOpenBrace !== -1 && lastOpenBrace > lastCloseBrace;

      if (selectedOperator) {
        if (insideBraces) {
          insertText = selectedOperator + '#' + itag + ' ';
        } else {
          insertText = selectedOperator + '{#' + itag + '} ';
        }
      } else {
        const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
        if (insideBraces) {
          insertText = (needsSpace ? ' ' : '') + '#' + itag + ' ';
        } else {
          insertText = (needsSpace ? ' ' : '') + '{#' + itag + '} ';
        }
      }
    }

    // ==================== MODE: QUERY ====================
    else if (searchMode === "where") {
      if (selectedOperator) {
        // Operator selected
        insertText = ' ' + selectedOperator + 'table.includes(_.itags, \'' + itag + '\')';
      } else {
        // No operator: determine if "and" is needed
        const trimmed = valueBeforeCursor.substring(0, cursorPos).trim();
        const needsAnd = trimmed !== '' && !trimmed.endsWith('and') && !trimmed.endsWith('or');
        if (needsAnd) {
          insertText = 'table.includes(_.itags, \'' + itag + '\')';
        } else {
          insertText = 'table.includes(_.itags, \'' + itag + '\')';
        }
      }
    }

    // ==================== MODE: UPDATE ====================
    else if (searchMode === "update") {
      // Not applicable for itags in update mode
      // Just insert the itag with #
      const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
      insertText = (needsSpace ? ' ' : '') + '#' + itag;
    }

    // ==================== MODE: SORT ====================
    else if (searchMode === "sort") {
      // Not really applicable for sorting by itags, but insert anyway
      const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
      insertText = (needsSpace ? ' ' : '') + '#' + itag;
    }

    // ==================== MODE: CALC ====================
    else if (searchMode === "calc") {
      // Not applicable for calc mode, but insert anyway
      const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
      insertText = (needsSpace ? ' ' : '') + '#' + itag;
    }

    // ==================== MODE: STAT ====================
    else if (searchMode === "stat") {
      // Not applicable for stat mode, but insert anyway
      const needsSpace = cursorPos > 0 && valueBeforeCursor[cursorPos - 1] !== ' ';
      insertText = (needsSpace ? ' ' : '') + '#' + itag;
    }

    // Build new value
    const newValue = valueBeforeCursor.substring(0, cursorPos) +
                     insertText +
                     valueBeforeCursor.substring(cursorPos);

    searchInput.value = newValue;

    // Move cursor after insertion
    const newCursorPos = cursorPos + insertText.length;
    searchInput.setSelectionRange(newCursorPos, newCursorPos);

    // Clear state
    searchInput._slashPosition = null;
    searchInput._selectedOperator = null;

    // Trigger appropriate action if in list mode
    if (searchMode === "list") {
      sessionStorage.setItem("searchTermFilter", newValue);
      applySearchFilter(newValue);
    }

    searchInput.focus();
  }

  // --- Operators per mode ---

  function getOperatorsForMode(mode) {
    if (mode === "list") {
      return [
        { label: "space", value: " " },
        { label: "+", value: "+" },
        { label: "-", value: "-" }
      ];
    } else if (mode === "where") {
      return [
        { label: "and", value: "and" },
        { label: "or", value: "or" },
        { label: "not", value: "not" }
      ];
    } else if (mode === "update") {
      return [
        { label: "add:", value: "add:" },
        { label: "delete:", value: "delete:" },
        { label: "rename:", value: "rename:" }
      ];
    } else if (mode === "sort") {
      return [
        { label: "+", value: "+" },
        { label: "-", value: "-" }
      ];
    } else if (mode === "calc") {
      return [
        { label: "+", value: "+" },
        { label: "-", value: "-" },
        { label: "->", value: "->" }
      ];
    } else if (mode === "stat") {
      return [];
    }

    return [];
  }

  // !!!=========================================
  // 15. UPDATE MODE — Collect tasks and execute
  // ============================================

  function getVisibleTaskIds() {
    const taskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');
    const taskIds = [];
    taskDivs.forEach(function(div) {
      if (window.getComputedStyle(div).display !== 'none') {
        const taskId = div.getAttribute('data-task-id');
        if (taskId) {
          taskIds.push(taskId);
        }
      }
    });
    return taskIds;
  }

  async function executeAttributeUpdate(command) {
    // Parser: "add:priority=high" or "delete:tag" or "rename:old->new"
    const colonIndex = command.indexOf(':');
    if (colonIndex === -1) {
      await syscall('editor.flashNotification', 'Invalid syntax. Use: add:name[=value] | delete:name | rename:old->new');
      return;
    }

    const action = command.substring(0, colonIndex).trim();
    const details = command.substring(colonIndex + 1).trim();

    if (!['add', 'delete', 'rename'].includes(action)) {
      await syscall('editor.flashNotification', 'Invalid action. Use: add, delete, or rename');
      return;
    }

    const taskIds = getVisibleTaskIds();
    if (taskIds.length === 0) {
      await syscall('editor.flashNotification', 'No tasks visible');
      return;
    }

    let attrName = '';
    let newValue = '';

    if (action === 'add') {
      const equalIndex = details.indexOf('=');
      if (equalIndex === -1) {
        attrName = details;
        newValue = '';
      } else {
        attrName = details.substring(0, equalIndex).trim();
        newValue = details.substring(equalIndex + 1).trim();
      }
    } else if (action === 'rename') {
      const arrowIndex = details.indexOf('->');
      if (arrowIndex === -1) {
        await syscall('editor.flashNotification', 'Invalid rename syntax. Use: rename:old->new');
        return;
      }
      attrName = details.substring(0, arrowIndex).trim();
      newValue = details.substring(arrowIndex + 2).trim();
    } else {
      attrName = details;
    }

    if (!attrName) {
      await syscall('editor.flashNotification', 'Attribute name required');
      return;
    }

    // Run Lua command
    const taskIdsJson = JSON.stringify(taskIds);
    await syscall("editor.invokeCommand", "TaskExplorer: UpdateAttributes", [
        taskIds,
        action,
        attrName,
        newValue
    ]);
  }

  // !!!=========================================
  // 15b. STAT MODE — Compute totals and counts
  // ============================================

  function executeStatOnTasks(command) {
    var attrNames = command.split(/\s+/).filter(function(s) { return s.length > 0; });
    if (attrNames.length === 0) {
      syscall('editor.flashNotification', 'Enter one or more attribute names');
      return;
    }

    var taskDivs = document.querySelectorAll('.sb-TaskExplorer-div-task');
    var results = [];

    attrNames.forEach(function(attrName) {
      var total = 0;
      var count = 0;

      taskDivs.forEach(function(taskDiv) {
        if (window.getComputedStyle(taskDiv).display === 'none') return;
        var val = extractAttributeValue(taskDiv, attrName);
        if (val !== null && val !== undefined && val !== '' && val !== '..') {
          // Exclude date-like values (YYYY-MM-DD, DD/MM/YYYY, etc.)
          if (/^\d{4}[-\/]\d{2}[-\/]\d{2}/.test(val)) return;
          if (/^\d{2}[-\/\.]\d{2}[-\/\.]\d{4}/.test(val)) return;
          var num = parseFloat(val);
          if (!isNaN(num)) {
            total += num;
            count++;
          }
        }
      });

      var openBracket = String.fromCharCode(91);
      var closeBracket = String.fromCharCode(93);
      // Round to avoid floating point artifacts
      var displayTotal = (total % 1 === 0) ? total : Math.round(total * 100) / 100;
      results.push(openBracket + attrName + closeBracket + ' ∑ ' + displayTotal + ' (' + count + ' tasks)');
    });

    var resultLine = results.join(' | ');
    window.exportTaskList(resultLine);
  }

  // !!!=========================================
  // 16. CONFIRMATION MODAL
  // ============================================
  parent.window.showUpdateConfirmationModal = function(logText, confirmationMessage, callId) {

      // Create the HTML of the modal
      // NOTE: inline styles required here because modal is injected into parent.document,
      // which does not have access to TCstyles.css (loaded only in the iframe)
      const modalHTML = `
        <div id="updateConfirmModal" style="
          position: fixed;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          background-color: rgba(0, 0, 0, 0.5);
          display: flex;
          justify-content: center;
          align-items: center;
          z-index: 10000;
        ">
          <div style="
            background: white;
            border-radius: 8px;
            padding: 20px;
            max-width: 600px;
            max-height: 80vh;
            display: flex;
            flex-direction: column;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
          ">
            <h3 style="margin: 0 0 15px 0; color: #333;">Update Confirmation</h3>

            <pre style="
              background: #f5f5f5;
              padding: 15px;
              border-radius: 4px;
              overflow-y: auto;
              max-height: 50vh;
              margin: 0 0 15px 0;
              font-family: monospace;
              font-size: 13px;
              line-height: 1.4;
            ">${logText}</pre>

            <div style="
              margin-bottom: 20px;
              color: #d97706;
            ">${confirmationMessage}</div>

            <div style="
              display: flex;
              gap: 10px;
              justify-content: flex-end;
            ">
              <button autofocus type="button" id="modalCancel" style="
                padding: 8px 20px;
                border: 1px solid #ccc;
                background: white;
                border-radius: 4px;
                cursor: pointer;
                font-size: 14px;
              ">Cancel</button>
              <button type="button" id="modalContinue" style="
                padding: 8px 20px;
                border: none;
                background: #2563eb;
                color: white;
                border-radius: 4px;
                cursor: pointer;
                font-size: 14px;
                font-weight: 500;
              ">Continue</button>
            </div>
          </div>
        </div>
      `;

      // Inject in the DOM
      const parentDoc = parent.document;
      parentDoc.body.insertAdjacentHTML('beforeend', modalHTML);
      const modal = parentDoc.getElementById('updateConfirmModal');
      const btnCancel = parentDoc.getElementById('modalCancel');
      const btnContinue = parentDoc.getElementById('modalContinue');

      // Extract the focus from the input of the iframe.
      // Attention: the focus is immediately captured by the editor.
      // So, a clic on Enter adds a line into the page opened in editor !
      btnCancel.focus();
      setTimeout(() => {
        parentDoc.getElementById('modalCancel')?.focus();
       }, 0);

      // Attach events to parent DOM elements
      btnContinue.onclick = () => {
        globalThis.syscall("event.dispatch", "updateConfirmResultTC", {
          id: callId,
          value: "yes"
        });
        modal.remove(); // clean modal
        };
      btnCancel.onclick = () => {
        globalThis.syscall("event.dispatch", "updateConfirmResultTC", {
          id: callId,
          value: "no"
        });
        modal.remove(); // clean modal
      };
  };

  // !!!=========================================
  // 17. INITIALISATION
  // ============================================
  if (document.readyState === "loading") {
    document.addEventListener("DOMContentLoaded", function () {
      restoreScrollPosition();
    });
  } else {
    setTimeout(function () {
      restoreScrollPosition();
    }, 50);
  }

  const searchInput = document.getElementById("tileSearch");
  if (!searchInput) return;

  showMenuForCurrentMode(searchInput, "true");
  initializeSearchMenu(searchInput);
  updateStatusBar();

  // Sync attrDisplay-btn with body class
  (function() {
    var attrBtn = document.getElementById('attrDisplay-btn');
    if (attrBtn && document.body.classList.contains('attr-alt-display')) {
      attrBtn.classList.add('active');
    }
  })();

  const inputSearch = document.querySelector('#tileSearch');
  document.querySelectorAll('.mode-switcher div').forEach(el => el.classList.remove('active'));
  searchModeEnCours = sessionStorage.getItem("searchMode");
    if (searchModeEnCours === "list") {
      const btn = document.querySelector('#listMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 real-time filter (Esc to exit)";
      searchInput.className = "mode-list";
    } else if (searchModeEnCours === "where") {
      const btn = document.querySelector('#whereMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 clause where (query with Enter or Esc to exit)";
      searchInput.className = "mode-where";
    } else if (searchModeEnCours === "update") {
      const btn = document.querySelector('#updateMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 add:name[=value] | delete:name | rename:old->new (Enter to execute)";
      searchInput.className = "mode-update";
    } else if (searchModeEnCours === "sort") {
      const btn = document.querySelector('#sortMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 attr1[+|-][ attr2+|-][ !] to keep all (Enter to sort)";
      searchInput.className = "mode-sort";
    } else if (searchModeEnCours === "calc") {
      const btn = document.querySelector('#calcMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 a [+|-] b [+|-] c... -> result (Enter to calculate)";
      searchInput.className = "mode-calc";
    } else if (searchModeEnCours === "stat") {
      const btn = document.querySelector('#statMode');
      btn.classList.add('active');
      inputSearch.placeholder = "\u00A0\u00A0\u00A0\u00A0...\u00A0\u00A0 attr1 attr2... (Enter to compute stats)";
      searchInput.className = "mode-stat";
    }
})();

  // -------- Opening task with UnifiedAdvancedPanelControl -------------
  window.openWithUnifiedPanel = async function(internalPath) {
    try {
        // Set the suppression flag so the new window doesn't spawn an explorer2
        await syscall('clientStore.set', 'explorer2.suppressOnce', 'true');

        // Extract the file name with the @position suffix
        const fileName = internalPath.split('/').pop();
        //.replace(/@\d+$/, '');

        // Build the Lua command to show the unified panel
        const luaCmd = `js.import("/.fs/Library/Mr-xRed/UnifiedAdvancedPanelControl.js").show("${internalPath}", "${fileName}")`;

        // Execute the Lua command
        await syscall('lua.evalExpression', luaCmd);
    } catch (error) {
        console.error('Error opening with unified panel:', error);
        // Fallback to standard navigation if something goes wrong
        await syscall('editor.navigate', internalPath, false, false);
    }
  };

]]

  -- ----------------- SHOW -----------------
  editor.showProgress(90, "PANEL ...")
  js.window.setTimeout(function()
    editor.showPanel(PANEL_ID, currentWidth, finalHtml, script)
  end, 100)
  editor.showProgress()
  PANEL_VISIBLE = true
  clientStore.set("explorer2.open", "true")
end

-- ----------------- COMMANDS ------------------

command.define {
  name = "TaskExplorer: Change View Mode",
  hide = true,
  run = function(args)
    if args.mode ~= "grid" then
      clientStore.set(VIEW2_MODE_KEY, args.mode)
      local clauseWhereInSession = ""
      clauseWhereInSession = js.window.sessionStorage.getItem("searchTerm")
      drawPanel("maj", clauseWhereInSession)
    else
      editor.flashNotification("mode inconnu !")
    end
  end
}

command.define {
  name = "TaskExplorer: Toggle Opening Mode",
  hide = true,
  run = function()
    local current = clientStore.get("explorer2.disableFilter")
    if current == "true" then
      clientStore.set("explorer2.disableFilter", "false")
    else
      clientStore.set("explorer2.disableFilter", "true")
    end
    drawPanel("maj")
  end
}

command.define {
  name = "Navigate: Task Commander",
  hide = true,
  run = function()
    if PANEL_VISIBLE then
      editor.hidePanel(PANEL_ID)
      PANEL_VISIBLE = false
      clientStore.set("explorer2.open", "false")
    else
      -- Reset
      js.window.sessionStorage.setItem("searchMode", "list")
      js.window.sessionStorage.setItem("searchInit", "true")
      js.window.sessionStorage.setItem("searchTermInit", WHERE_INIT)
      js.window.sessionStorage.setItem("searchTerm", WHERE_INIT)
      js.window.sessionStorage.setItem("searchTermFilter", "")
      -- Execution
      drawPanel()
    end
  end
}

command.define {
  name = "Navigate: Toggle Task Commander",
  key = "Ctrl-Alt-v",
  run = function()
    if PANEL_VISIBLE then
      editor.hidePanel(PANEL_ID)
      PANEL_VISIBLE = false
    else
      local lastMode = clientStore.get("explorer2.currentDisplayMode") or "panel"
      if lastMode == "window" then
        editor.invokeCommand("Navigate: Task Commander Window")
      else
        editor.invokeCommand("Navigate: Task Commander")
      end
    end
  end
}

command.define {
  name = "Navigate: Task Commander Window",
  hide = true,
  run = function()
    local selector = "#sb-main .sb-panel." .. PANEL_ID
    if not PANEL_VISIBLE then
      --Reset
      js.window.sessionStorage.setItem("searchMode", "list")
      js.window.sessionStorage.setItem("searchInit", "true")
      js.window.sessionStorage.setItem("searchTermInit", WHERE_INIT)
      js.window.sessionStorage.setItem("searchTerm", WHERE_INIT)
      js.window.sessionStorage.setItem("searchTermFilter", "")
      -- Execution
      drawPanel()
    end
    clientStore.set("explorer2.open", "true")
    js.import("/.fs/Library/Mr-xRed/UnifiedAdvancedPanelControl.js").enableWindow(selector)
  end
}

command.define {
  name = "Open: Instructions",
  hide = false,
  run = function()
    js.import("/.fs/Library/Mr-xRed/UnifiedAdvancedPanelControl.js").show(
      "Library/baudogit/TCreference")
  end
}

command.define {
  name = "Task: Toggle_Done",
  hide = true,
  run = function(args)
    local param1 = args[1]
    local param2 = args[2]
    local param3 = args[3]
    local param4 = args[4]
    toggleTaskRemote(param1, param2, param3, param4)
  end
}

command.define {
  name = "TaskExplorer: ExecuteQuery",
  hide = true,
  run = function(args)
    local param1 = args[1]
    local param2 = args[2]
    drawPanel(param1, param2)
  end
}

command.define {
  name = "TaskExplorer: UpdateAttributes",
  hide = true,
  run = function(args)
    --js.log(args)
    local taskIds = args[1]
    local action = args[2]
    local attributeName = args[3]
    local newValue = args[4] or ""
    updateTaskAttributes(taskIds, action, attributeName, newValue)
  end
}

command.define {
  name = "TaskExplorer: CalcBatchUpdate",
  hide = true,
  run = function(args)
    local taskIds = args[1]
    local attrName = args[2]
    local values = args[3]
    local logText = args[4]
    local confirmMessage = args[5]

    if not taskIds or #taskIds == 0 then
      editor.flashNotification("No tasks to update")
      return
    end

    -- REQUEST FOR CONFIRMATION (same pattern as updateTaskAttributes)
    local callId = tostring(os.time() .. math.random())

    pendingModals[callId] = function(value)
      if value ~= "yes" then
        editor.flashNotification("Calculation cancelled")
        return
      end

      -- REAL TREATMENT
      local count = calcBatchUpdate(taskIds, attrName, values)

      js.window.setTimeout(function()
        local whereClause = ""
        local savedSearchTerm = js.window.sessionStorage.getItem("searchTerm");
        if savedSearchTerm ~= nil and savedSearchTerm ~= "" then
          whereClause = savedSearchTerm
        end
        drawPanel("maj", whereClause)
        editor.flashNotification(count .. " task(s) calculated: " .. attrName)
      end, 100)
    end

    js.window.showUpdateConfirmationModal(logText, confirmMessage, callId)
  end
}

command.define {
  name = "TaskExplorer: Export task list",
  hide = true,
  run = function(args)
    local taskIds = args[1]
    local ch = args[2] or ""
    if not taskIds or #taskIds == 0 then
      editor.flashNotification("No tasks to export")
      return
    end
    local dateExtract = os.date('%Y-%m-%d %H-%M-%S')
    datastore.set({ "TEtaskList", "1" }, { text = ch, taskList = taskIds, date = dateExtract })
    editor.invokeCommand("Task: TC export")
    -- key is not deleted ("Task: TC export" allows to re-edit it manually)
  end
}

```


```space-lua
-- ::: space-lua 2 :::

-- ---------- BUILD QUERY ---------

function queryWithParam (clauseWhere)

  -- To VARY a VALUE in the WHERE clause (example)
  -- local queryTxt = { where = lua.parseExpression("_.done == optionValue") }
  -- local results = index.queryLuaObjects(tagName, queryTxt, { optionValue = true } )
  
  -- !! This does not work (os.time) !!
  --clauseWhere = some(_.completed) ~= nil and string.sub(_.completed,1,10) >= (os.date('%Y-%m-%d', os.time() - (5 * 24 * 60 * 60)))
    --clauseWhere = some(_.completed) ~= nil and (string.sub(_.completed,1,10) >= os.date('%Y-%m-%d',  os.time({ year = 2025, month = 12, day = 21 })))

  --_.completed>='2026-01-17 22:02'

  -- To test: modify and uncomment below. This squizzle all the parameters.
  -- clauseWhere = "_.done == not true or _.done == true"

  -- Check the validity of the where clause
  local statusWhere, validWhere = pcall(function() 
        return lua.parseExpression(clauseWhere) 
  end)
  if not statusWhere then
    editor.flashNotification("Invalid where clause !")  
    return {}  
  end

  local tagName = "task"  
  local queryTxt = {  
    where = validWhere,
    orderBy = {
        {
          expr = lua.parseExpression("page"),
          desc = false
          },
        {
          expr = lua.parseExpression("pos"),
          desc = false
          }
      }
  }  
  --local results = index.queryLuaObjects(tagName, queryTxt)  
  local status, results = pcall(function() 
        return index.queryLuaObjects(tagName, queryTxt)  
  end)
  if status then  
    return js.tolua(results)  
  else   
    editor.flashNotification("The query returns an error !")  
    return {}  
  end
end

-- ---------- BUILD HTML ---------

function tasksByPage(allTasks, viewMode, clauseWhere)
  local queryTasks = {}
  
  -- Wait for asynchronous indexing to complete
  mq.awaitEmptyQueue("indexQueue")
  
  -- Extracting tasks
  if allTasks then  
    -- BASE: queryTasks = query[[from index.tag 'task' order by page, pos]]
    queryTasks = queryWithParam(clauseWhere)
  else
    if clauseWhere == "" or clauseWhere == nil then
      clauseWhere = " _.done == not true"
    else
      clauseWhere = clauseWhere .. " and _.done == not true"
    end
    -- BASE: queryTasks = query[[from index.tag 'task' where not done order by page, pos]]
    queryTasks = queryWithParam(clauseWhere)
  end

  --js.log(queryTasks)

   -- Break down tasks by page
  local pageTasks = {}
  local countTasks = 0
  for task in queryTasks do
    if not pageTasks[task.page] then
      pageTasks[task.page] = {}
    end
    table.insert(pageTasks[task.page], task)
    countTasks = countTasks + 1
  end

  -- Build html with templates
  local html = ""
  if viewMode == "list" then
    local nb = 1
    local nbT = #queryTasks
    for pageName, tasks in pairs(pageTasks) do
      html = html .. pageTasksTemplate({
        tasks = tasks
      })
      nb = nb + #tasks
      editor.showProgress(math.floor((nb/nbT)*100), "mon-type")
    end
  else
    local nb = 0
    local nbT = #queryTasks
    for pageName, tasks in pairs(pageTasks) do
      html = html .. pageTasksPerPageTemplate({
        pageName = pageName,
        tasks = tasks
      })
      nb = nb + #tasks
      editor.showProgress(math.floor((nb/nbT)*100), "mon-type")
    end
  end
  return {html, countTasks}
end

-- ---------- TEMPLATES ---------

-- Template with page name and task
local lib = " task "
-- (#tasks > 1 and " tasks " or " task ") 
local pageTasksPerPageTemplate = template.new[==[
${formatPage(pageName .. "<br><small>" .. #tasks .. " ".. "extracted</small>")}
${template.each(tasks, templates.taskItem3)}
]==]

-- Template with task only
local pageTasksTemplate = template.new[==[
${template.each(tasks, templates.taskItem3)}
]==]

-- Template for a task (format)
templates.taskItem3 = template.new([==[ ${ formatTask(ref .. "###" .. tostring(done) .. "###" .. state .. "###" .. tostring(itags) .. "###" .. text)} ]==])

```


```space-lua
-- ::: space-lua 3 :::

-- ---------- FORMAT PAGE ---------

function formatPage(pageString)

  -- Div with a specific class for the page name
  local ps = '<div class= "sb-TaskExplorer-div-page">'
    ps = ps .. '<span class= "sb-TaskExplorer-span-page">' .. pageString .. '</span>'
    ps = ps .. '</div >'
  return ps
end

-- ---------- FORMAT TASK ---------

function formatTask(taskString)
    if not taskString or taskString == "" then
        return ""
    end
  local ICONS = {
    square = '<svg class="icon-svg" style="width: 1em; height: 1em; vertical-align: middle;" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="18" x="3" y="3" rx="2"/></svg>',
    squarecheck ='<svg class="icon-svg" style="width: 1em; height: 1em; vertical-align: middle;" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect width="18" height="18" x="3" y="3" rx="2"/><path d="m9 12 2 2 4-4"/></svg>',
  }
  
    local reference = ""
    local done = "false"
    local doneReal = ""
    local status = ""
    local stateExist = ""
    local itags = ""
    local tempString = taskString
    local tempStringInit = taskString
    local sepPos = taskString:find("###", 1, true)
    local positionStart = ""
    local namePage = ""
    local listArgs = {}
    local nameIcon = ""
    local viewMode = clientStore.get("modeView") or "list"
  
    -- ---------- Analyse STRING ---------
  
    if sepPos then
        reference = taskString:sub(1, sepPos - 1)
        local remaining = taskString:sub(sepPos + 3)

        -- Find the second separator ###
        local sepPos2 = remaining:find("###", 1, true)
        if sepPos2 then
            done = remaining:sub(1, sepPos2 - 1)
            local remaining2 = remaining:sub(sepPos2 + 3)
            
            -- Find the third separator ###
            local sepPos3 = remaining2:find("###", 1, true)
            if sepPos3 then
                status = remaining2:sub(1, sepPos3 - 1)
                local remaining3 = remaining2:sub(sepPos3 + 3)
                
                -- Find the fourth separator ###
                local sepPos4 = remaining3:find("###", 1, true)
                if sepPos4 then
                    itags = remaining3:sub(1, sepPos4 - 1)
                    tempString = remaining3:sub(sepPos4 + 3)
                else
                    tempString = remaining3
                end
            else
                tempString = remaining2
            end
        else
            tempString = remaining
        end
    else
        reference = taskString:match("^([^%s]+)") or ""
        tempString = taskString:gsub("^[^%s]+%s+", "")
    end

    -- Extracting the name to display (after the last /)
    local displayName = reference
    if reference:find("/", 1, true) then
        displayName = reference:match("([^/]+@%d+)") or reference
    end

    -- Extracting the label (everything not in brackets)
    local label = ""
    -- Extract all parts outside the brackets
    local parts = {}
    local j = 1
    local currentPart = ""
    local inBracket = false
    
    while j <= #tempString do
        local char = tempString:sub(j, j)

        if char == "[" and not inBracket then
            -- Start of an attribute
            if currentPart ~= "" then
                -- Normalize multiple spaces into one
                local normalized = currentPart:gsub("%s+", " ")
                normalized = normalized:match("^%s*(.-)%s*$") or normalized
                if normalized ~= "" then
                    table.insert(parts, normalized)
                end
                currentPart = ""
            end
            inBracket = true
        elseif char == "]" and inBracket then
            -- End of an attribute
            inBracket = false
        elseif not inBracket then
            -- Add character to label
            currentPart = currentPart .. char
        end
        j = j + 1
    end
    -- Add the last part if it exists
    if currentPart ~= "" then
        local normalized = currentPart:gsub("%s+", " ")
        normalized = normalized:match("^%s*(.-)%s*$") or normalized
        if normalized ~= "" then
            table.insert(parts, normalized)
        end
    end
    -- Join parts with " | "
    if #parts > 0 then
        label = table.concat(parts, " | ")
    end
    -- Add "status" if custom
    if status ~= " " and status ~= "x" and status ~= "X" then  
      label = "[" .. status .. "] ".. label
      stateExist = status
    end

    -- Function to escape HTML special characters
    local function escapeHtml(text)
        local replacements = {
            ["&"] = "&amp;",
            ["<"] = "&lt;",
            [">"] = "&gt;",
            ['"'] = "&quot;",
            ["'"] = "&#39;",
            ["/"] = "&#47;"
        }
        return (text:gsub("[&<>\"'/]", replacements))
    end

    label = escapeHtml(label)

    -- Extracting attributes [key:value]
    local attributes = {}
    local i = 1

    while i <= #taskString do
        local startBracket = taskString:find("%[", i)
        if not startBracket then
            break
        end

        local endBracket = taskString:find("%]", startBracket)
        if not endBracket then
            -- Hook not closed, we ignore the rest
            break
        end
        local attr = taskString:sub(startBracket + 1, endBracket - 1)
        local key, value = attr:match("^%s*([^:]+):%s*(.*)%s*$")

        if key and value then
            -- Clean spaces
            local cleanKey = key:match("^%s*(.-)%s*$")
            local cleanValue = value:match("^%s*(.-)%s*$")
            key = cleanKey or key
            value = cleanValue or value
            table.insert(attributes, { key = key, value = value })
        end

        i = endBracket + 1
    end

    -- ----------- Building HTML -------------

    -- Enclosing DIV
    local divClass = 'sb-TaskExplorer-div-task'
    if done == "true" then
        divClass = divClass .. ' cm-task-checked'
    end
  
    -- Construct the unique task ID
    local taskId = ""
    if reference:find("@", 1, true) then
        -- Extract namePage and position from reference
        local pageNameFromRef = reference:match("(.-)@")
        local posFromRef = reference:match("@(%d+)")
        if pageNameFromRef and posFromRef then
            local adjustedPos = tostring(tonumber(posFromRef))
            taskId = pageNameFromRef .. '@' .. adjustedPos
        end
    end

      tempStringInit = tempString .. "\n" .. taskId .. "       ITAGS: " .. itags

    -- Add data-task-id to div
    local html = '<div class="' .. divClass .. '" data-task-id="' .. taskId .. '">'

    -- Enclosing SPAN
    html = html .. '<span class="sb-TaskExplorer-span-task sb-list sb-task">'
  
    -- If the reference contains an @, provides
    -- full path (with position+ 2), position+2 and page name
    local nextRef = reference
    if reference:find("@", 1, true) then
        nextRef = reference:gsub("@(%d+)", function(num)
            return "@" .. tostring(tonumber(num) + 2)
        end)
        positionStart = reference:match("@(%d+)")
        positionStart = tostring(tonumber(positionStart) + 2)
      namePage = reference:match("(.-)@")
    end
  
    -- Toggle status task (clic on icon)
    if done == "true" then
      nameIcon = ICONS.squarecheck
      doneReal = "X"
    else
      nameIcon = ICONS.square
      doneReal = " "
    end  
    -- schema : html (onclick) > js (script) > syscall (invokeCommand) > space-lua (function)
    html = html .. '<span style="cursor: pointer;" onclick="toggleTask(\'' ..   
    string.gsub(namePage or "", "'", "\\'") .. '\',' ..   
    (positionStart or "0") .. ',\'' ..   
    string.gsub(doneReal or "", "'", "\\'") .. '\',\'' ..  
    string.gsub(stateExist or "", "'", "\\'") .. '\')">'
    html = html .. nameIcon .. '</span> '

    -- Enclosing DIV  
    -- 1st line: label with link to open the task page 
    if reference:find("@", 1, true) then
      local optionView = clientStore.get("explorer2.disableFilter")
      html = html .. '<div id="divTaskchild" title="' .. escapeHtml(tempStringInit) .. '" onclick='
      if optionView == "true" then
        -- Open in iframe
        html = html .. '"openWithUnifiedPanel(\'' .. reference .. '\')" >' .. label
      else
        -- Open in editor windows
        html = html .. '"syscall(\'editor.navigate\', \'' .. nextRef .. '\', false, false)" >' .. label 
      end
    else
        -- No link
        html = html .. escapeHtml(displayName) .. ' '
    end

    --  attributes not empty
    if next(attributes) ~= nil then
      html = html .. '<br /> '
    end

    -- 2nd line: attributes. Original structure is preserved with span empty (class: sb-task)
    -- between each attribute. Text is compiled on the 1st ligne (label).
    for _, attr in ipairs(attributes) do
        html = html .. '<span class="sb-list sb-task"> </span>'
        html = html .. '<span class="sb-attribute" data-' .. attr.key .. '="' .. escapeHtml(attr.value) .. '">'
        html = html .. '<span class="sb-list sb-frontmatter sb-meta">[</span>'
        -- depending to SilverBullet version date
        local dateVersion = js.window.sessionStorage.getItem("dateVersion")
        if dateVersion >= "2026-02-06" then
          html = html .. '<span class="sb-list sb-frontmatter sb-attribute-name">' .. escapeHtml(attr.key) .. '</span>'
        else
          html = html .. '<span class="sb-list sb-frontmatter sb-atom">' .. escapeHtml(attr.key) .. '</span>'
        end
        html = html .. '<span class="sb-list sb-frontmatter sb-meta">: </span>'
        html = html .. '<span class="sb-list sb-frontmatter sb-attribute-value">' .. escapeHtml(attr.value) .. '</span>'
        html = html .. '<span class="sb-list sb-frontmatter sb-meta">]</span>'
        html = html .. '</span>'
    end

    if viewMode == "list" then
        html = html .. '<br /> ' .. taskId
    end
    html = html .. "</div></span></div>"

    return html
end

```
