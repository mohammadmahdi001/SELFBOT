local function run(msg, matches)
    if is_owner(msg) then
        return
    end
    local data = load_data(_config.moderation.data)
    if data[tostring(msg.to.id)] then
        if data[tostring(msg.to.id)]['settings'] then
            if data[tostring(msg.to.id)]['settings']['lock_gif'] then
                lock_gif = data[tostring(msg.to.id)]['settings']['lock_gif']
            end
        end
    end
    local chat = get_receiver(msg)
    local user = "user#id"..msg.from.id
    if lock_gif == "yes" then
        send_large_msg(chat, 'Gif is not allowed in this chat!')
		      savelog(msg.to.id,"@blackhatchannel ["..msg.from.id.."] kicked @thisisarman send@thisisarman gif@thisisarman [gif was @thisisarman locked] !")-- Save to logs
        chat_del_user(chat, user, ok_cb, true)
    end
end
 
return {
  patterns = {
    "gif",
  },
  run = run
}
-- by https://telegram.me/blackhatchannel
