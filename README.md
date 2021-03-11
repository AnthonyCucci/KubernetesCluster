# Kubernetes Cluster on Raspberry Pi's

This project is to experiment with 3d printing and developing a low powered Kubernetes cluster on raspberry Pi’s. As I am creating/printing the cluster case itself, I want to include a switch, power, and space for the raspberry Pi’s in a compact and portable manor.

## Requirements:
1 Ender 3 pro (or similar 3 printer)

3x Raspberry Pi 3

1x TP link 8 port switch unmanaged switch https://www.amazon.com/Ethernet-Splitter-Optimization-Unmanaged-TL-SG108/dp/B00A121WN6

1x Anker 60w 6 port USB charging station amazon.com/Anker-Charger-PowerPort-iPhone-Galaxy/dp/B00P936188/

3x Right angle Micro USB Cable https://www.amazon.com/SUNGUY-3-Pack-Degree-Charging-Samsung/dp/B07H172CHD/

3x .5 FT Ethernet cables https://www.monoprice.com/product?p_id=3426

1x Ubiquiti USG (Or any router will work) https://www.ui.com/unifi-routing/usg/

## Printing

I started to made a very basic mockup on Autodesk fusion 360:

<img src="https://user-images.githubusercontent.com/47187712/109887461-b96b2700-7c4f-11eb-8bdd-c58f1cb02fa9.jpg" width="500" height="400">

I then started to print individual pieces to make sure they would fit properly.

<img src="https://user-images.githubusercontent.com/47187712/109887688-2c749d80-7c50-11eb-879d-cb370bec29d7.jpg" width="300" height="300">
This is the first prototype, not enough space for the whole pi to fit sadly...

Here is the first design to fit (had to cut the top off for the PI's):

<img src="https://user-images.githubusercontent.com/47187712/110850077-627fd600-827d-11eb-94c7-ff53f5788cf3.jpg" width="400" height="400">

Now all we need is the software!!


### Network:
My network is split up in the 10.10.10.0 subnet and split the names by network cable for easier diagnosing

<img src="https://user-images.githubusercontent.com/47187712/110851715-657bc600-827f-11eb-8bfa-5d5ef6eed73d.png">



### Changelog:
1.0.0
        Created Readme, Licenese, progresslog, and TODO

1.0.1
        Added to Requirments
        
1.1.0
        Added Printing information
        
1.1.1
        Added network diagram
        Added More printing information
