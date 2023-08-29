# Crazyflie Firmware  [![CI](https://github.com/bitcraze/crazyflie-firmware/workflows/CI/badge.svg)](https://github.com/bitcraze/crazyflie-firmware/actions?query=workflow%3ACI)

This project contains the source code for the firmware used in the Crazyflie range of platforms, including the Crazyflie 2.X and the Roadrunner.

### Crazyflie 1.0 support

The 2017.06 release was the last release with Crazyflie 1.0 support. If you want
to play with the Crazyflie 1.0 and modify the code, please clone this repo and
branch off from the 2017.06 tag.

## Building and Flashing
See the [building and flashing instructions](https://github.com/bitcraze/crazyflie-firmware/blob/master/docs/building-and-flashing/build.md) in the github docs folder.


## Official Documentation

Check out the [Bitcraze crazyflie-firmware documentation](https://www.bitcraze.io/documentation/repository/crazyflie-firmware/master/) on our website.

## Modification

The CrazyFlie wasn't stable enough at the first time. So we tried to do something to enhance the performance    


1. The anchor needs to be 15 cm from the ground and ceiling and also get away from the metal stuff.

2. The anchor from ceiling doesn't have the enough power to trasmit package in somtimes, which is the mainly reason of the oscillating in z-axis
Solution:  
2.1 follow [this link](https://www.bitcraze.io/documentation/repository/lps-node-firmware/master/development/anchor-low-level-config/) to enter the low level configuration:  

2.2 enhance tx power to 33.5 db by [this link](https://forum.bitcraze.io/viewtopic.php?p=21669&sid=f1358beaaf9d7acde30db62908c172c6#p21669)  

3. Initially, the loco had some big jump during estimating, so I follow [this instruction](https://github.com/bitcraze/crazyflie-firmware/issues/1158) to modify the outlier filter, now it is already upgraded to the main branch. 

4. In `kalman_core.c`, The estimation would differ from the `isflying` state. However, we can not tell the cf whether it is flying. 
so I just commented this function. Be careful of the acceleration updating. In drone assumption, we ignore the x axis and y axis.

5. I change some params in `kalman_core.c`. Search Hao modified to see what I've changed.

[Solve the yaw decay problem](https://github.com/bitcraze/crazyflie-firmware/blob/aaeaf11658ca8b3891b67f187fbc941d6aadd7bc/src/modules/src/kalman_core/kalman_core.c#L73)
Set the parameter to zero since the crazyflie can't tell whether it's flying.

## Inter-ranging while in TDOA 
For CLAMOUR project, we need to have the ranging data with each crazyflie, I follow [this thread](https://gist.github.com/krichardsson/c1a51642d2d560d825f7bee8577ac6c6) to modify the `lpsTdoa3Tag.c`. Setting an unused ID in the tdoa3.hmId parameter and setting tdoa3.hmTwr=1. TDoA can be turned off by setting tdoa3.hmTdoa=0. The distance can be obtained by creating the new log parameter in [here](https://gist.github.com/krichardsson/c1a51642d2d560d825f7bee8577ac6c6#file-lpstdoa3tag-c-L253). I lost this code version in my old ubuntu OS.. But I think it is very easy to bring it back by following the [disscusion between me and the author](https://github.com/bitcraze/crazyflie-firmware/issues/576#issuecomment-1533602187). Now we can get the ranging data, but the TDOA logging needs to be modified to get all of the data.    

## Lps Node Sniffer Mode
https://www.bitcraze.io/documentation/repository/lps-node-firmware/master/user-guides/tdoa3_setup/

## Generated documentation

The easiest way to generate the API documentation is to use the [toolbelt](https://github.com/bitcraze/toolbelt)

```tb build-docs```

and to view it in a web page

```tb docs```

## Contribute
Go to the [contribute page](https://www.bitcraze.io/contribute/) on our website to learn more.

### Test code for contribution

To run the tests please have a look at the [unit test documentation](https://www.bitcraze.io/documentation/repository/crazyflie-firmware/master/development/unit_testing/).

## License

The code is licensed under LGPL-3.0
