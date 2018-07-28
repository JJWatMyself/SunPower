# SunPower
This project is for monitoring SunPower solar using PRTG with Perl. Notes in reverse chronology are below

2018/07/28 Some optimizations are now in place:
1) At night, Inverters go into an error state and the reported remain frozen until state returns to working, e.g. degC reads 30 from dusk to dawn which is simply incorrect. The script will now change these values to zero.  In itself, zero may not be the correct value. I'd prefer there be nulls, but that's for another day. I don't have a screenshot of this yet as I made the changes at around 1 AM and it looks a bit messy right now.

2) State has values two values that I am aware of so far (working and error).  PRTG doesn't do words, just numbers, so I am reporting these as 0 (working), 1 (error) or 5 (unknown). 5 is completely made up and signifies that state is a value that is unknown to the script.  I then created a custom lookup that maps these integers to words in the UI... Yes, you guessed it: working; error; and unknown. These are mapped to up, warning and down/error. If you are new to PRTG, warning and down are threshold based alarms that change the color from green to either yellow or red.  These can be used for notifications.  In it's present state a notification would not make sense as would be generated every day after sunset.  However, if an inverter went into error state during the day, or if all went into error state during the day a notification would be useful.  TBD if I'll build this in or not and if that would be a function of the script or PRTG config.  Here is a screenshot that shows the result of the loopkups.

What's next: more tweaks based on what I see.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept2.png)

2018/07/26 The POC completed on 7/25 took the serial number provided for any inverter and returned all the values for it. The data returned is interesting.  Now I've added logic to take the serial number provided, determine the device type first (Inverter, Power Meter, Supervisor), and then return the appropriate values for it along with the appropriate labels.  Here is a screenshot showing the single script being used to retrieve values for three device types. The plan is to let this sit, gather data for a couple of days and then work to refine the script. No code has been posted yet.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept1.png)

2018/07/25 Proof of concept has been completed. Working to understand all the different elements next and to formulate an overall monitoring template. Note that because this is an evening project, the returned values are for 'night-time' therefore no energy being produced when the screenshot was created.  :) No code has been posted yet.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept.png)
