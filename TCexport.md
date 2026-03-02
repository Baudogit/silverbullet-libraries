---
command: "Task: TC export"
suggestedName: "TCexport_${os.date('%Y-%m-%d %H-%M-%S')}"
description: A page template to show the contents of tasks exported by Task Commander
confirmName: false
tags: meta/template/page
---
Click here: ${"$"}{widgets.button("Show task list", function()  (function() templates.TCtaskItem = template.new([==[
[${"$"}{state}] [[${"$"}{ref}]]
${"$"}{text}
]==]) local refsForPage = datastore.get({ "TEtaskList", "1" }) local dateExtract = refsForPage.date local refs = refsForPage.taskList  local clauseWhere = "table.includes(refs, ref)" local validWhere = lua.parseExpression(clauseWhere) local queryTxt = {  where = validWhere, orderBy = {{expr = lua.parseExpression("page"),desc = false},{expr = lua.parseExpression("pos"), desc = false}}} local results = index.queryLuaObjects("task", queryTxt, {refs = refs}) local queryStr = js.tolua(results)  local ch = "**" .. refsForPage.text .. "**\n\n"  local result = ch .. template.each(queryStr, templates.TCtaskItem) local timeGenerate = os.date('%Y-%m-%d %H-%M-%S') local str = "List generated on " .. timeGenerate .. "\nfrom the ID extracted on " .. dateExtract .. " with the criteria below.\n" editor.moveCursorToLine(7, 1, false) editor.insertAtCursor(str) result = result .. "\n----\n" editor.moveCursorToLine(10, 1, false) editor.insertAtCursor(result) editor.setSelection(1, 500) editor.deleteLine() editor.moveCursorToLine(1, 1, false) local secondButton = "${"$"}{widgets.button('Toggle attribute highlighting', function() (function() local body = js.window.document.body if not body then editor.flashNotification('Body inaccessible') return end local classStr = body.className or '' local hasAlt = string.find(classStr, 'attr%-alt%-display') and true or false if hasAlt then body.className = string.gsub(classStr, '%s*attr%-alt%-display', '') else body.className = classStr .. ' attr-alt-display' return end end)() end)}\n" editor.insertAtCursor(secondButton) return  end)() end)}
|^|   
---



