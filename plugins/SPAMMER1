do

function run(msg, matches)
       if not is_sudo(msg) then
              return " "       end
  local tex = matches[1]
  local sps = matches[2]
  local sp = 'done by BlackLifeTM'

for i=1, tex, 1 do

sp = '\n'..sps..'\n'..sp
i = i + 1

end

return sp

end

return {
       description = "send large msg",
        usage = {
                 sudo = {
                    "!spam : send large msg to chat" },
             },
    patterns = {

      "[!/#]spam (.*) (.*)$"
    },
    run = run,
}

end
