# SunPower
This project is for monitoring SunPower solar using PRTG with Perl

2018/07/25 Proof of concept has been completed. Working to understand all the different elements next and to formulate an overall monitoring template. Note that becasue this is an evening project, the returned values are for 'night-time' therefore no energy being produced when the screenshot was created.  :) No code has been posted yet.

![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept.png)

2018/07/26 The POC completed on 7/25 took the serial number provided for any inverter and returned all the values for it. The data returned is interesting.  Now I've added logic to take the serial number provided, determine the device type (Inverter, Power Meter, Supervisor), and then return the appropriate values for it.  Here is a screenshot showing the single script being used to retrive values for three device types. The plan is to let this sit and gether data for a couple of days and then work to refine the script. No code has been posted yet.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept1.png)
