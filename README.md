# PTZ-jortan jt-8177

This custom component allows you to use the Pan and Tilt functions of Chinese cameras that do not fully comply with the ONVIF protocol.

You will have to enable the connection and set a password in the Yoosee app under camera -> Settings -> NVR connection -> Enable to connect

To use this custom component, I recommend integrating the video stream from your cameras with motionEye.
**Please note that this component does not handle the video stream from the cameras, only the pan and tilt functions**

### Tested Cameras

  - [jortan jt-8177]
  
![](camara360.jpg)
  
      - rtsp://IP:554/onvif1
      - rtsp://IP:554/onvif2

### [OPTIONAL] How to get the stream uri from ONVIF cameras

  #### this will get the profile token that will be needed for the next step
      - curl -X POST -H "Content-Type: application/soap+xml" -d @GetProfile.xml http://192.168.1.244:5000/onvif/device_service 
  #### Update GetStreamUri.xml with your ProfileToken and post the request to get the stream uri
      - curl -X POST -H "Content-Type: application/soap+xml" -d @GetStreamUri.xml http://192.168.1.244:5000/onvif/media_service
      
## Installation
You only need to install the custom component in the usual way. Copy the "ptz_camera" folder from this project into your Home Assistant's /config/custom_components/ directory.

## Configuration
In your configuration.yaml file:

```yaml
ptz_camera:
```
## Services
This custom component creates several services under the domain ptz_camera. To access information about these services, you can use "Developer Tools" > "Services" in Home Assistant. Here, you will find detailed information about the arguments required to call each service.

## Camera Entity
You can create a camera in the usual way. I recommend using the [motionEye](https://addons.community/) addon to create an MJPEG camera. It's the best configuration I have found with low delay. An example of the configuration would be:

```yaml
camera:
  - platform: mjpeg
    name: Salon
    mjpeg_url: http://192.168.1.111:8083
```

## Card with Controls
An easy way to use the pan and tilt controls is to overlay them on the image of a camera. Replace the IP address in this example with the IP address of your camera.

```yaml
type: picture-elements
camera_view: live
camera_image: camera.salon
elements:
  - type: icon
    icon: 'mdi:arrow-left-drop-circle'
    tap_action:
      action: call-service
      service: ptz_camera.move_left
      service_data:
        host: 192.168.1.244
    style:
      bottom: 45%
      left: 5%
      color: white
      opacity: 0.5
      transform: 'scale(1.5, 1.5)'
  - type: icon
    icon: 'mdi:arrow-right-drop-circle'
    tap_action:
      action: call-service
      service: ptz_camera.move_right
      service_data:
        host: 192.168.1.244
    style:
      bottom: 45%
      right: 5%
      color: white
      opacity: 0.5
      transform: 'scale(1.5, 1.5)'
  - type: icon
    icon: 'mdi:arrow-up-drop-circle'
    tap_action:
      action: call-service
      service: ptz_camera.move_up
      service_data:
        host: 192.168.1.244
    style:
      top: 10%
      left: 46%
      color: white
      opacity: 0.5
      transform: 'scale(1.5, 1.5)'
  - type: icon
    icon: 'mdi:arrow-down-drop-circle'
    tap_action:
      action: call-service
      service: ptz_camera.move_down
      service_data:
        host: 192.168.1.244
    style:
      bottom: 10%
      left: 46%
      color: white
      opacity: 0.5
      transform: 'scale(1.5, 1.5)'
  - type: icon
    icon: 'mdi:arrow-expand-all'
    tap_action:
      action: more-info
    entity: camera.salon
    style:
      top: 5%
      right: 5%
      color: white
      opacity: 0.5
      transform: 'scale(1.5, 1.5)'

```
      
![](tarjeta.jpg)
