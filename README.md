# SWIFI-4KFLOCAM
 Automating Swann 4K Floodlight Security Camera with 2-Way Talk, Siren with Heat &amp; Motion Detection.

Product Company : Swann

Product ID : SWIFI-4KFLOCAM

Product Description : 4K Floodlight Security Camera with 2-Way Talk, Siren with Heat & Motion Detection.

#### Testing the Swann C.C.T.V Camera API (Working)

##### Get the current config

Input
```
curl http://my-swann-cctv-ip-address/API10/getMediaConfig
```

Output
```
{ "Live Video Quality": 3, "Image Mirror": 0, "Image Flip": 0, "Video Environment": 60, "Video Environment Mode": 2, "Mic Volume": 50, "Speaker Volume": 85, "Light": 0, "Light On Motion": 1, "Light On Motion Duration": 120, "Light On Manual Duration": 120, "Siren": 0, "Siren On Motion": 0, "Siren On Motion Duration": 0, "Siren On Manual Duration": 120, "Enforcer": 0, "Enforcer On Motion": 0, "Enforcer On Motion Duration": 0, "Enforcer On Manual Duration": 30, "Light Intensity": 10 }
```

##### Change the current config

Input
```
curl -d '{"Speaker Volume":"80"}' -H "Content-Type: application/json" -X POST http://my-swann-cctv-ip-address/API10/setMediaConfig
```

Output
```
{ "Speaker Volume": 80, "result": "success" }
```

##### Check the new config

Input
```
curl http://my-swann-cctv-ip-address/API10/getMediaConfig
```

Output
```
{ "Live Video Quality": 3, "Image Mirror": 0, "Image Flip": 0, "Video Environment": 60, "Video Environment Mode": 2, "Mic Volume": 50, "Speaker Volume": 80, "Light": 0, "Light On Motion": 1, "Light On Motion Duration": 120, "Light On Manual Duration": 120, "Siren": 0, "Siren On Motion": 0, "Siren On Motion Duration": 0, "Siren On Manual Duration": 120, "Enforcer": 0, "Enforcer On Motion": 0, "Enforcer On Motion Duration": 0, "Enforcer On Manual Duration": 30, "Light Intensity": 10 }
```

#### Testing the Swann C.C.T.V Camera RTSP Stream for [Frigate NVR](https://github.com/blakeblackshear/frigate) (working)

##### View the camera stream

Input
```
ffplay -fs rtsp://my-swann-cctv-ip-address:554
```

Output
```
Stream #0:0: Video: hevc (Main), 3840x2160, 15 fps, 15 tbr, 90k tbn
```
