do
local function dl_read(chat_id, target)
	local f = io.open("./quran/Sura"..target..".mp3","r")
	if f ~= nil then
		io.close(f)
		return send_document("channel#id"..chat_id,"./quran/Sura"..target..".mp3", ok_cb, false)
	else
		return 'مقدار وارد شده موجود نیست'
	end
end

local function view_sura(chat_id, target)
	local f = io.open("./quran/quran ("..target..").txt","r")
	if f ~= nil then
		local text = f:read("*all")
		return text
	else
		return 'مقدار وارد شده موجود نیست'
	end
end

local function run(msg, matches)
    local chat_id = msg.to.id
	if matches[1] == "ead" then
		local target = matches[2]
		return dl_read(chat_id, target)
	elseif matches[1] == "ura" then
		local target = matches[2]
		return view_sura(chat_id, target)
	elseif matches [1] == "uran" then
		local file = io.open("./quran/list.txt", "r")
		local text = file:read("*all")
		return "لیست سوره های موجود:\n\n"..text.."\n______________________________\nبرای مشاهده ی سوره و دریافت فایل صوتی آن، فقط عدد سوره را وارد کنید\nsura عدد\nread عدد"
	end
end

return {
	description = "Quran", 
	usagehtm = '<tr><td align="center">quran</td><td align="right">ليست سوره ها را همراه عددشان نمايش ميدهد. دقت کنید فقط جزء 30 قرآن موجود است به همراه سوره ی حمد و آیت الکرسی</td></tr>'
	..'<tr><td align="center">sura عدد</td><td align="right">نمایش متن یک سوره</td></tr>'
	..'<tr><td align="center">read عدد</td><td align="right">پخش قرائت سوره</td></tr>',
	usage = {
		"sura (num) : نمایش سوره",
		"read (num) : قرائت",
		"quran : لیست سوره ها",
	},
	patterns = {
		"^[Ss](ura) (.+)$",
		"^[Rr](ead) (.+)$",
		"^[Qq](uran)$",
    }, 
	run = run,
	}
end
