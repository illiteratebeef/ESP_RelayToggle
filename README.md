This is a program to toggle a pin on and off whenever the device recieved any kind of http request request. It's best to run this on an ESP8266 with the most recent NodeMCU firmware and using ESPlorer to program it. 

Bugs fixed: 
removed old file.writeline formatting
To Do:
Server crashes after ~5 minutes, make it not do that.


wifi.setmode(wifi.STATION);
wifi.sta.config("Wifi SSID","Wifi Password");
status=0
pin=1
gpio.mode(pin,gpio.OUTPUT)
srv=net.createServer(net.TCP) 
srv:listen(80,function(conn)
	conn:on("receive",function(conn,payload)
		-- print(payload)
		if string.find(payload,"favicon.ico") == nil then
		if status==0 then gpio.write(pin,gpio.HIGH); status=1 else gpio.write(pin,gpio.LOW); status=0 end
		end
		conn:send("<h1> The relay status is " .. status ..".</h1>")
		end)
	conn:on("sent",function(conn) conn:close() end)
end)
