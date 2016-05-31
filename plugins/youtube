--AIzaSyAErvLuowcEpnfSNbAcZj3E1WELJpgvMgc
do

local function httpsRequest(url)
  print(url)
  local res,code  = https.request(url)
  if code ~= 200 then return nil end
  return json:decode(res)
end
--https://www.googleapis.com/youtube/v3/search?part=snippet&maxResults=4&type=video&key=AIzaSyAErvLuowcEpnfSNbAcZj3E1WELJpgvMgc&q=
local function searchYoutubeVideos(text)
  local url = 'https://www.googleapis.com/youtube/v3/search?'
  url = url..'part=snippet'..'&maxResults=4'..'&type=video&key=AIzaSyAErvLuowcEpnfSNbAcZj3E1WELJpgvMgc'
  url = url..'&q='..URL.escape(text)
  local data = httpsrequest(url)

  if not data then
    print("HTTP Error")
    return nil
  elseif not data.items then
    return nil
  end
  return data.items
end

local function run(msg, matches)
  local text = ''
  local items = searchYoutubeVideos(matches[1])
  if not items then
    return "#Error\nخطا مشکلی پیش آمده است"
  end
  for k,item in pairs(items) do
    text = text..'http://youtu.be/'..item.id.videoId..' '..
      item.snippet.title..'\n\n'
  end
  return text
end

return {
  description = "Search video on youtube and send it.",
  usage = "!youtube [term]: Search for a youtube video and send it.",
  patterns = {
    "^[/#!]youtube (.*)"
  },
  run = run
}

end
