# 📚 Linoria ( Custom ) Library - เอกสารการใช้งานฉบับสมบูรณ์ (ภาษาไทย)
Linoria Library เป็น UI Library สำหรับ Roblox ที่มีประสิทธิภาพสูง รองรับฟีเจอร์ครบครัน ใช้งานง่าย และมีระบบจัดการธีมและการบันทึกค่าอัตโนมัติ
```
-- โหลด Library จาก GitHub repository
local repo = 'https://raw.githubusercontent.com/violin-suzutsuki/LinoriaLib/main/'
local Library = loadstring(game:HttpGet(repo .. 'Library.lua'))()
local ThemeManager = loadstring(game:HttpGet(repo .. 'addons/ThemeManager.lua'))()
local SaveManager = loadstring(game:HttpGet(repo .. 'addons/SaveManager.lua'))()

-- สร้างหน้าต่างหลัก
local Window = Library:CreateWindow({
    Title = 'ชื่อ Script ของคุณ',
    Center = true,                -- จัดกลางหน้าจอ
    AutoShow = true,              -- แสดง UI ทันทีที่สร้าง
    TabPadding = 8,               -- ระยะห่างระหว่าง Tab
    MenuFadeTime = 0.2            -- เวลาในการแสดง/ซ่อนเมนู
})
-- สร้าง Tab ต่างๆ
local Tabs = {
    Main = Window:AddTab('Main'),
    Visual = Window:AddTab('Visual'),
    Combat = Window:AddTab('Combat'),
    ['UI Settings'] = Window:AddTab('UI Settings')
}
-- Groupbox ด้านซ้ายและขวา
local LeftGroupBox = Tabs.Main:AddLeftGroupbox('การตั้งค่าทั่วไป')
local RightGroupBox = Tabs.Main:AddRightGroupbox('การตั้งค่าขั้นสูง')
-- สร้าง Tabbox
local TabBox = Tabs.Main:AddLeftTabbox('กลุ่มตั้งค่า')

-- เพิ่มแท็บใน Tabbox
local Tab1 = TabBox:AddTab('แท็บ 1')
local Tab2 = TabBox:AddTab('แท็บ 2')
LeftGroupBox:AddToggle('MyToggle', {
    Text = 'เปิดใช้งานฟีเจอร์',
    Default = false,
    Tooltip = 'คำอธิบายเพิ่มเติม',
    Risky = false,  -- ถ้าเป็น true จะใช้สีแดง (สำหรับฟีเจอร์เสี่ยง)
    Callback = function(Value)
        print('Toggle เปลี่ยนค่าเป็น:', Value)
    end
})

-- การใช้งาน Toggle ในภายหลัง
Toggles.MyToggle:OnChanged(function()
    print('ค่า Toggle ปัจจุบัน:', Toggles.MyToggle.Value)
end)

-- เปลี่ยนค่าด้วยโค้ด
Toggles.MyToggle:SetValue(true)
LeftGroupBox:AddSlider('MySlider', {
    Text = 'ความเร็ว',
    Default = 50,
    Min = 0,
    Max = 100,
    Rounding = 1,          -- จำนวนทศนิยม (0 = จำนวนเต็ม)
    Suffix = '%',          -- หน่วยต่อท้าย
    Compact = false,       -- ไม่แสดงชื่อ
    HideMax = false,       -- ไม่แสดงค่าสูงสุด
    Callback = function(Value)
        print('Slider เปลี่ยนค่าเป็น:', Value)
    end
})

-- การใช้งาน
Options.MySlider:OnChanged(function()
    print('ค่า Slider ปัจจุบัน:', Options.MySlider.Value)
end)

Options.MySlider:SetValue(75)
-- Dropdown ปกติ
LeftGroupBox:AddDropdown('MyDropdown', {
    Values = {'ตัวเลือก 1', 'ตัวเลือก 2', 'ตัวเลือก 3'},
    Default = 1,           -- เลือกตัวเลือกที่ 1 เป็นค่าเริ่มต้น
    Multi = false,         -- ไม่ให้เลือกหลายค่า
    Text = 'เลือกตัวเลือก',
    AllowNull = true,      -- อนุญาตให้ไม่เลือกอะไรเลย
    Callback = function(Value)
        print('เลือก:', Value)
    end
})

-- Multi Dropdown (เลือกได้หลายค่า)
LeftGroupBox:AddDropdown('MultiDropdown', {
    Values = {'A', 'B', 'C', 'D'},
    Default = 1,
    Multi = true,          -- เลือกได้หลายค่า
    Text = 'เลือกหลายตัว',
    Callback = function(Value)
        print('ตัวเลือกทั้งหมด:', Value)
    end
})

-- Player Dropdown (เลือกผู้เล่นอัตโนมัติ)
LeftGroupBox:AddDropdown('PlayerDropdown', {
    SpecialType = 'Player',  -- โหลดรายชื่อผู้เล่นอัตโนมัติ
    Text = 'เลือกผู้เล่น',
    AllowNull = true,
    Callback = function(Value)
        print('เลือกผู้เล่น:', Value)
    end
})

-- การใช้งาน
Options.MyDropdown:SetValue('ตัวเลือก 2')
Options.MultiDropdown:SetValue({A = true, C = true})
-- ต้องสร้างจาก Toggle หรือ Label
LeftGroupBox:AddToggle('ColorToggle', {
    Text = 'เลือกสี'
}):AddColorPicker('MyColor', {
    Default = Color3.fromRGB(255, 0, 0),  -- สีแดงเป็นค่าเริ่มต้น
    Title = 'เลือกสีของคุณ',              -- ชื่อของ ColorPicker
    Transparency = 0,                     -- ความโปร่งใส (0-1)
    Callback = function(Color)
        print('เลือกสี:', Color)
    end
})

-- หรือสร้างจาก Label
LeftGroupBox:AddLabel('สีพื้นหลัง'):AddColorPicker('BgColor', {
    Default = Color3.fromRGB(0, 100, 255),
    Transparency = 0.5,  -- โปร่งใส 50%
})

-- การใช้งาน
Options.MyColor:OnChanged(function()
    print('สีปัจจุบัน:', Options.MyColor.Value)
    print('ความโปร่งใส:', Options.MyColor.Transparency)
end)

Options.MyColor:SetValueRGB(Color3.fromRGB(0, 255, 0))
-- สร้างจาก Toggle
LeftGroupBox:AddToggle('KeybindToggle', {
    Text = 'ปุ่มลัด'
}):AddKeyPicker('MyKeybind', {
    Default = 'Z',                -- ปุ่มเริ่มต้น
    Mode = 'Toggle',              -- โหมด: 'Toggle', 'Hold', 'Always'
    SyncToggleState = false,      -- ไม่ sync กับ Toggle state
    Text = 'ปุ่มเปิด/ปิดฟีเจอร์',
    NoUI = false,                 -- แสดงใน Keybind menu
    Callback = function(Value)
        print('ปุ่มลัดถูกกด:', Value)
    end,
    ChangedCallback = function(NewKey)
        print('เปลี่ยนปุ่มลัดเป็น:', NewKey)
    end
})

-- การใช้งาน
Options.MyKeybind:OnClick(function()
    print('กดปุ่มลัด! สถานะ:', Options.MyKeybind:GetState())
end)

Options.MyKeybind:OnChanged(function()
    print('ปุ่มลัดถูกเปลี่ยนเป็น:', Options.MyKeybind.Value)
end)

-- ตรวจสอบสถานะใน Loop
game:GetService('RunService').Heartbeat:Connect(function()
    if Options.MyKeybind:GetState() then
        print('กำลังกดปุ่มลัดค้างอยู่')
    end
end)

-- เปลี่ยนปุ่มลัดด้วยโค้ด
Options.MyKeybind:SetValue({ 'MB2', 'Hold' })  -- เปลี่ยนเป็น Mouse Button 2 โหมด Hold
LeftGroupBox:AddInput('MyInput', {
    Default = 'ข้อความเริ่มต้น',
    Numeric = false,          -- อนุญาตเฉพาะตัวเลข
    Finished = false,         -- เรียก Callback เมื่อกด Enter เท่านั้น
    Text = 'กรอกข้อความ',
    Placeholder = 'พิมพ์ที่นี่...',
    MaxLength = 50,           -- จำกัดความยาว
    Tooltip = 'คำอธิบายช่องกรอก',
    Callback = function(Value)
        print('กรอกข้อความ:', Value)
    end
})

-- การใช้งาน
Options.MyInput:OnChanged(function()
    print('ข้อความปัจจุบัน:', Options.MyInput.Value)
end)
-- ปุ่มกดธรรมดา
local MyButton = LeftGroupBox:AddButton({
    Text = 'กดฉัน',
    Func = function()
        print('กดปุ่มแล้ว!')
    end,
    DoubleClick = false,      -- ต้องดับเบิ้ลคลิก
    Tooltip = 'คำอธิบายปุ่ม'
})

-- ปุ่มย่อย (Sub Button)
local SubButton = MyButton:AddButton({
    Text = 'ปุ่มย่อย',
    Func = function()
        print('กดปุ่มย่อย!')
    end,
    DoubleClick = true,       -- ต้องดับเบิ้ลคลิก
    Tooltip = 'ปุ่มย่อย'
})

-- การเชื่อมต่อหลายปุ่ม (Chaining)
LeftGroupBox:AddButton({ Text = 'ฟังก์ชัน 1', Func = function() print('1') end })
    :AddButton({ Text = 'ฟังก์ชัน 2', Func = function() print('2') end })
    :AddButton({ Text = 'ฟังก์ชัน 3', Func = function() print('3') end })
-- ข้อความธรรมดา
LeftGroupBox:AddLabel('นี่คือข้อความธรรมดา')

-- ข้อความแบบ Wrap (ขึ้นบรรทัดใหม่อัตโนมัติ)
LeftGroupBox:AddLabel('นี่คือข้อความยาวๆ\nที่ใช้ \\n เพื่อขึ้นบรรทัดใหม่\n\nหรือให้ Library Wrap ให้อัตโนมัติ', true)

-- Label พร้อม Addon
local ColorLabel = LeftGroupBox:AddLabel('เลือกสี')
ColorLabel:AddColorPicker('LabelColor', {
    Default = Color3.fromRGB(255, 255, 0),
    Callback = function(Color)
        print('เลือกสีจาก Label:', Color)
    end
})
LeftGroupBox:AddDivider()  -- เพิ่มเส้นคั่น
-- สร้าง Toggle หลัก
RightGroupBox:AddToggle('MainFeature', {
    Text = 'เปิดฟีเจอร์หลัก',
    Default = false
})

-- สร้าง Dependency Box
local DepBox = RightGroupBox:AddDependencyBox()

-- เพิ่ม Element ลงใน Dependency Box
DepBox:AddSlider('FeatureSlider', {
    Text = 'ปรับค่าฟีเจอร์',
    Default = 50,
    Min = 0,
    Max = 100,
    Rounding = 0
})

DepBox:AddDropdown('FeatureDropdown', {
    Values = {'โหมด 1', 'โหมด 2', 'โหมด 3'},
    Default = 1,
    Text = 'เลือกโหมด'
})

-- ตั้งค่าเงื่อนไข: แสดงเมื่อ MainFeature เป็น true
DepBox:SetupDependencies({
    { Toggles.MainFeature, true }
})

-- Dependency Box ซ้อนกัน
local SubDepBox = DepBox:AddDependencyBox()
SubDepBox:AddToggle('SubFeature', {
    Text = 'ฟีเจอร์ย่อย',
    Default = false
})

SubDepBox:SetupDependencies({
    { Toggles.FeatureToggle, true }  -- ฟีเจอร์ย่อยจะแสดงเมื่อ FeatureToggle เป็น true
})
-- เปิดใช้งาน Watermark
Library:SetWatermarkVisibility(true)

-- ตั้งค่า Watermark แบบไดนามิก
local FrameTimer = tick()
local FrameCounter = 0
local FPS = 60

local WatermarkConnection = game:GetService('RunService').RenderStepped:Connect(function()
    FrameCounter = FrameCounter + 1
    
    if (tick() - FrameTimer) >= 1 then
        FPS = FrameCounter
        FrameTimer = tick()
        FrameCounter = 0
    end
    
    Library:SetWatermark(('Script Name | %s fps | %s ms'):format(
        math.floor(FPS),
        math.floor(game:GetService('Stats').Network.ServerStatsItem['Data Ping']:GetValue())
    ))
end)
-- แสดง Keybind Menu
Library.KeybindFrame.Visible = true
-- แสดงการแจ้งเตือน
Library:Notify('นี่คือการแจ้งเตือน', 5)  -- แสดง 5 วินาที
-- ส่งต่อ Library ให้ ThemeManager
ThemeManager:SetLibrary(Library)

-- ตั้งค่าโฟลเดอร์สำหรับธีม
ThemeManager:SetFolder('MyScriptHub')

-- นำธีมไปใช้กับ Tab
ThemeManager:ApplyToTab(Tabs['UI Settings'])

-- หรือใช้กับ Groupbox เฉพาะ
-- ThemeManager:ApplyToGroupbox(Groupbox)
-- ส่งต่อ Library ให้ SaveManager
SaveManager:SetLibrary(Library)

-- ไม่บันทึกค่าที่เกี่ยวข้องกับธีม
SaveManager:IgnoreThemeSettings()

-- ไม่บันทึกบางค่าที่กำหนด
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })

-- ตั้งค่าโฟลเดอร์สำหรับการบันทึก
SaveManager:SetFolder('MyScriptHub/GameName')

-- สร้างส่วนตั้งค่า Config ใน Tab
SaveManager:BuildConfigSection(Tabs['UI Settings'])

-- โหลด Config อัตโนมัติ
SaveManager:LoadAutoloadConfig()
local MenuGroup = Tabs['UI Settings']:AddLeftGroupbox('Menu')

-- ปุ่มปิด UI
MenuGroup:AddButton('Unload', function()
    Library:Unload()
end)

-- ตั้งค่าปุ่มเปิด/ปิดเมนู
MenuGroup:AddLabel('ปุ่มเปิด/ปิดเมนู'):AddKeyPicker('MenuKeybind', {
    Default = 'End',
    NoUI = true,  -- ไม่แสดงใน Keybind menu
    Text = 'ปุ่มเปิด/ปิดเมนู'
})

-- ตั้งค่าให้ Library ใช้ Keybind นี้
Library.ToggleKeybind = Options.MenuKeybind
```
## 📝 Tips และ Best Practices
1. การจัดการ Callback
```
-- วิธีที่แนะนำ: ใช้ OnChanged แยกจาก UI Creation
Toggle:OnChanged(function(Value)
    -- Logic code ที่นี่
end)

-- มากกว่า: ใส่ Callback ใน Options
{
    Callback = function(Value)
        -- อาจทำให้โค้ดรก
    end
}
```
