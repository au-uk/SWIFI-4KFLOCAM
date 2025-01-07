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

Note: Codec Issues Install ( No decoder could be found for codec hevc )

```
sudo dnf install --skip-unavailable ffplay libavcodec-freeworld gstreamer1-vaapi gstreamer1-plugins-base gstreamer1-plugins-good gstreamer1-plugins-good-extras gstreamer1-plugins-ugly-free gstreamer1-plugins-bad-free gstreamer1-libav gstreamer1-plugins-bad-freeworld gstreamer1-plugins-ugly gstreamer1-plugin-openh264 pipewire-gstreamer svt-vp9-libs x265 x265-lib

```

#### Use Ansible, [SemaphoreUI](https://github.com/semaphoreui/semaphore) and Git to automate the settings on all CCTV cameras (in-progress)

Note: The assumption is that all CCTV cameras will use the same settings. The settings file is stored in Git. The playbook runs periodically (or via webhook, if local git) and pulls the, main branch, setting file, from Git and applies the settings to an individual camera, group of cameras, or all cameras on your local network (Note: based on the Ansible Hosts File and the hosts: setting in the playbook).

##### Playbook

Input

```
---
    - name: POST Configuration to Swann CCTV
      hosts: swann_cctv_endpoints
      gather_facts: no
      vars:
        json_file_url: "https://raw.githubusercontent.com/au-uk/SWIFI-4KFLOCAM/main/MediaConfigData.json"
    
      tasks:
        - name: Fetch JSON file
          uri:
            url: "{{ json_file_url }}"
            return_content: yes
          register: json_content
    
        - name: Send POST request to API endpoints
          uri:
            url: "http://{{ swann_cctv_endpoints }}"
            method: POST
            body: "{{ json_content.content | from_json }}"
            body_format: json
            status_code: 200
          register: api_response
        
```
To use this playbook:

1. Define your API endpoints in the Ansible inventory file:

```ini
[swann_cctv_endpoints]
cctv1.example.com
cctv2.example.com
```

2. Run the playbook:

```bash
ansible-playbook -i inventory_file Swann-CCTV-MediaConfig-Ansible-Playbook.yml
```

Output

```
WIP
```