--set--
local function save_value(msg, name, value)
  if (not name or not value) then
    return 
  end
  local hash = nil
  if msg.to.type == 'channel' then
    hash = 'chat:'..msg.to.id..':variables'
  end
    if msg.to.type == 'chat' then
    hash = 'chat:'..msg.to.id..':variables'
  end
  if msg.to.type == 'user' then
    hash = 'user:'..msg.from.id..':variables'
  end
  if hash then
    redis:hset(hash, name, value)
    return "Saved "..name.." 👉 "..value
  end
end
--get--
local function get_variables_hash(msg)
  if msg.to.type == 'channel' then
    return 'chat:'..msg.to.id..':variables'
  end
    if msg.to.type == 'chat' then
    return 'chat:'..msg.to.id..':variables'
  end
  if msg.to.type == 'user' then
    return 'user:'..msg.from.id..':variables'
  end
end 
local function list_variables(msg)
  local hash = get_variables_hash(msg)
  
  if hash then
    local names = redis:hkeys(hash)
    local text = ''
    for i=1, #names do
      text = text..names[i]..'\n'
    end
    return text
  end
end
local function get_value(msg, var_name)
  local hash = get_variables_hash(msg)
  if hash then
    local value = redis:hget(hash, var_name)
    if not value then
      return
    else
      return value
    end
  end
end
--insta--
local access_token = "3084249803.280d5d7.999310365c8248f8948ee0f6929c2f02"
local function instagramUser(msg, query)
    local receiver = get_receiver(msg)
	local url = "https://api.instagram.com/v1/users/search?q="..URL.escape(query).."&access_token="..access_token
	local jstr, res = https.request(url)
	if res ~= 200 then
		return "No Connection"
    end
	local jdat = json:decode(jstr)
	if #jdat.data == 0 then
		send_msg(receiver,"#Error\nUsername not found",ok_cb,false)
	end
	if jdat.meta.error_message then
		send_msg(receiver,"#Error\n"..jdat.meta.error_message,ok_cb,false)
	end
	local id = jdat.data[1].id
	local gurl = "https://api.instagram.com/v1/users/"..id.."/?access_token="..access_token
	local ress = https.request(gurl)
	local user = json:decode(ress)
   	if user.meta.error_message then
		send_msg(receiver,"#Error\n"..user.meta.error_message,ok_cb,false)
	end
	local text = ''
	if user.data.bio ~= '' then
		text = text.."Username: "..user.data.username:upper().."\n\n"
	else
		text = text.."Username: "..user.data.username:upper().."\n"
	end
	if user.data.bio ~= '' then
		text = text..user.data.bio.."\n\n"
	end
	if user.data.full_name ~= '' then
		text = text.."Name: "..user.data.full_name.."\n"
	end
	text = text.."Media Count: "..user.data.counts.media.."\n"
	text = text.."Following: "..user.data.counts.follows.."\n"
	text = text.."Followers: "..user.data.counts.followed_by.."\n"
	if user.data.website ~= '' then
		text = text.."Website: "..user.data.website.."\n"
	end
	text = text
	local file_path = download_to_file(user.data.profile_picture,"insta.png")     -- disable this line if you want to send profile photo as sticker
	local file_path = download_to_file(user.data.profile_picture,"insta.webp")    -- enable this line if you want to send profile photo as sticker
	local cb_extra = {file_path=file_path}
    local mime_type = mimetype.get_content_type_no_sub(ext)
	--send_photo(receiver, file_path, rmtmp_cb, cb_extra)  -- disable this line if you want to send profile photo as sticker
  send_document(receiver, file_path, rmtmp_cb, cb_extra)  -- enable this line if you want to send profile photo as sticker
	send_msg(receiver,text,ok_cb,false)
end

local function instagramMedia(msg, query)
    local receiver = get_receiver(msg)
	local url = "https://api.instagram.com/v1/media/shortcode/"..URL.escape(query).."?access_token="..access_token
	local jstr, res = https.request(url)
	if res ~= 200 then
		return "No Connection"
    end
	local jdat = json:decode(jstr)
	if jdat.meta.error_message then
		send_msg(receiver,"#Error\n"..jdat.meta.error_type.."\n"..jdat.meta.error_message,ok_cb,false)
	end
	local text = ''
	local data = ''
	if jdat.data.caption then
	      data = jdat.data.caption
	      text = text.."Username: "..data.from.username:upper().."\n\n"
		  text = text..data.from.full_name.."\n\n"
		  text = text..data.text.."\n\n"
		  text = text.."Like Count: "..jdat.data.likes.count.."\n"
    else
	      text = text.."Username: "..jdat.data.user.username:upper().."\n"
		  text = text.."Name: "..jdat.data.user.full_name.."\n"
		  text = text.."Like Count: "..jdat.data.likes.count.."\n"
	end
	text = text
	send_msg(receiver,text,ok_cb,false)
end

--google--
local function googlethat(query)
  local api        = "http://ajax.googleapis.com/ajax/services/search/web?v=1.0&"
  local parameters = "q=".. (URL.escape(query) or "")

  -- Do the request
  local res, code = https.request(api..parameters)
  if code ~=200 then return nil  end
  local data = json:decode(res)

  local results = {}
  for key,result in ipairs(data.responseData.results) do
    table.insert(results, {
        result.titleNoFormatting,
        result.unescapedUrl or result.url
      })
  end
  return results
end

local function stringlinks(results)
  local stringresults=""
  for key,val in ipairs(results) do
    stringresults=stringresults..val[1].." - "..val[2].."\n"
  end
  return stringresults
end
local function run(msg, matches)
   if matches[1] == 'google' then
  local results = googlethat(matches[1])
  return 'نتایج جستجو\n\n'..stringlinks(results)
end
   if matches[1] == 'webshot' then
   local find = get_webshot_url(matches[2])
   if find then
      local imgurl = base .. find
      local receiver = get_receiver(msg)
      send_photo_from_url(receiver, imgurl)
   end
end
if matches[1] == 'sticker' then
  local texturl = "http://latex.codecogs.com/png.download?".."\\dpi{800}%20\\LARGE%20"..URL.escape(matches[2])
  local receiver = get_receiver(msg)
  local file = download_to_file(texturl,'text.webp')
      send_document('chat#id'..msg.to.id, file, ok_cb , false)
      send_document('channel#id'..msg.to.id, file, ok_cb , false)
end
   if matches[1] == 'voice' then
  local voiceapi = "http://tts.baidu.com/text2audio?lan=en&ie=UTF-8&text="..URL.escape(matches[2])
  local receiver = get_receiver(msg)
  local file = download_to_file(voiceapi,'text.ogg')
      send_audio('channel#id'..msg.to.id, file, ok_cb , false)
      send_audio('chat#id'..msg.to.id, file, ok_cb , false)
end
if matches[1] == "insta" and not matches[3] then
    return instagramUser(msg,matches[2])
end
if matches[1] == "insta" and matches[3] then
    local media = matches[3]
    if string.match(media , '/') then media = media:gsub("/", "") end
    return instagramMedia(msg,media)
end
if matches[1] == 'set' and is_sudo(msg) then
    local name = string.sub(matches[2], 1, 50)
  local value = string.sub(matches[3], 1, 1000)
  local text = save_value(msg, name, value)
  return text
end
if matches[1] == 'get' then
    if matches[2] then
    return get_value(msg, matches[2])
  else
    return list_variables(msg)
  end
end
end

return {
  description = "funny plugin",
  usage = "see commands in patterns",
  patterns = {
    "^(google) (.*)$",
    "^(webshot) (https?://[%w-_%.%?%.:/%+=&]+)$",
    "^(voice) (.+)$",
   "^[/#!](insta) ([Hh]ttps://www.instagram.com/p/)([^%s]+)$",
   "^[/#!](insta) ([Hh]ttps://instagram.com/p/)([^%s]+)$",
   "^[/#!](insta) ([Hh]ttp://www.instagram.com/p/)([^%s]+)$",
   "^[/#!](insta) ([Hh]ttp://instagram.com/p/)([^%s]+)$",
   "^[/#!](insta) (.*)$",    
   "^(set) ([^%s]+) (.*)$",
   "^(get) (.*)$",
   "^(sticker) (.*)$",
    },
  run = run
}
