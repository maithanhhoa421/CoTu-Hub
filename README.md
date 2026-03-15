# CoTu-Hub
Chào bạn! Dựa trên hình ảnh trò chơi Roblox mà bạn cung cấp, đây là một đoạn code Lua sử dụng thư viện UI đơn giản để thực hiện yêu cầu của bạn.

Lưu ý: Đoạn code này được thiết kế cho các phần mềm thực thi script (executor) trên Roblox. Vì mỗi trò chơi có cấu trúc nút bấm khác nhau, mình đã thiết lập để script tìm các nút dựa trên nội dung chữ (Text) hiển thị trên màn hình của bạn.

Đoạn Code Auto Câu Cá & Spam Chiêu
Lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Auto Fishing - Hoadauan", "Midnight")

-- Biến kiểm tra trạng thái
local _G.AutoFishing = false

-- Khởi tạo UI
local Tab = Window:NewTab("Main")
local Section = Tab:NewSection("Điều khiển")

Section:NewToggle("Bật/Tắt Auto", "Tự động câu cá và spam chiêu", function(state)
    _G.AutoFishing = state
    if state then
        print("Đã bật Auto")
        -- Luồng chạy song song để không làm treo máy
        spawn(function()
            while _G.AutoFishing do
                -- 1. Click nút "Câu cá"
                pcall(function()
                    -- Tìm nút có chứa chữ "Câu cá" hoặc "Cau ca"
                    for _, v in pairs(game:GetService("Players").LocalPlayer.PlayerGui:GetDescendants()) do
                        if v:IsA("TextLabel") or v:IsA("TextButton") then
                            if v.Text:find("Câu cá") or v.Text:find("Câu") then
                                -- Giả lập nhấn vào nút cha hoặc chính nó
                                local target = v:IsA("TextButton") and v or v.Parent
                                if target:IsA("GuiButton") then
                                    game:GetService("VirtualInputManager"):SendMouseButtonEvent(target.AbsolutePosition.X + target.AbsoluteSize.X/2, target.AbsolutePosition.Y + target.AbsoluteSize.Y/2, 0, true, game, 1)
                                    game:GetService("VirtualInputManager"):SendMouseButtonEvent(target.AbsolutePosition.X + target.AbsoluteSize.X/2, target.AbsolutePosition.Y + target.AbsoluteSize.Y/2, 0, false, game, 1)
                                end
                            end
                        end
                    end
                end)

                -- 2. Spam 4 chiêu và hiện thông báo
                for i = 1, 4 do
                    if not _G.AutoFishing then break end
                    
                    -- Gửi thông báo góc dưới phải
                    game:GetService("StarterGui"):SetCore("SendNotification", {
                        Title = "Action",
                        Text = "Press " .. i,
                        Duration = 0.5
                    })

                    -- Nhấn các phím 1, 2, 3, 4 tương ứng
                    game:GetService("VirtualInputManager"):SendKeyEvent(true, tostring(i), false, game)
                    task.wait(0.1)
                    game:GetService("VirtualInputManager"):SendKeyEvent(false, tostring(i), false, game)
                    
                    task.wait(0.2) -- Khoảng nghỉ giữa các chiêu
                end
                
                task.wait(0.5) -- Nghỉ một chút trước khi lặp lại vòng mới
            end
        end)
    else
        print("Đã tắt Auto")
    end
end)

-- Tạo hiệu ứng viền RGB cho UI (Tùy chọn thẩm mỹ)
spawn(function()
    while task.wait() do
        local Color = Color3.fromHSV(tick() % 5 / 5, 1, 1)
        -- Code này yêu cầu Library hỗ trợ đổi màu trực tiếp, 
        -- ở bản Midnight mặc định nó sẽ có màu tối sang trọng.
    end
end)
