# SunPower
This project is for monitoring SunPower solar using PRTG with Perl. Notes in reverse chronology are below

2018/08/01 The portion of the code that reports a specified field for all inverters, where the channel name is the inverter serial number, is now complete. This will be very interesting as provides a view of all inverters together and will show any differences in performance, be it due to environmental factors like angle, time of day shadows, snow or simply due to internal components. Note that the screenshots were created after the sun went down so the data is boring with a capital B.  In the morning it will come to life and as was the case with the first proof of concept screenshots, I would like to follow-up with more when there is a couple of days’ worth of data and perhaps will provide a two day historical view. The other thing that will likely be interesting is to see which inverter powers up first or powers off last each day.  My guess is that there will be a winner in each category.  If this data can be used as the basis of a warranty claim, then I'll claim credit now as I predict it will happen at some point. :) No code has been posted yet.

Command line parameters: inverters state
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept6.png)

Command line parameters: inverters t_htsnk_degc
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept7.png)


2018/07/30 I did some work yesterday morning, adding a new section of code that will output the field you specify for all inverters.  The name of the channel is the serial number. It's an effective piece of logic that has a simple method to determine how many inverters there are in the system. This code section provides a way to compare the inverters to see any differences in performance, be it due to environmentals, or due to the performance of the internal components.  Where I left off was incorporating sub-routines but seeing I have never really written perl before (well, I started a week or two ago now) there are some principles I have not researched yet.  I may end up having a couple duplicate code sections for now, just to keep the work on functionality moving.  I have also thought about another hybrid sensor that sums values from the inverters as a way to compare it to the Power Meter. In the meantime, now that there have been a couple of days of data-gathering, here are some screenshots. Interestingly there was a supervisor reboot this morning (uptime reset to zero) and it looks like it may forget about what inverters there are until they power up (sunlight).  No code has been posted yet.

Power Meter on the generation side, 2 day view (5 minute averages)
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept3.png)

Supervisor, 2 day view (5 minute averages)
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept4.png)

Inverter (Panel), 2 day view (5 minute averages)
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept5.png)

2018/07/28 Some optimizations are now in place:
1) At night, Inverters go into an error state and the reported values remain frozen until state returns to working, e.g. degC reads 30 from dusk to dawn which is simply incorrect. The script will now change these values to zero.  Zero may not be the correct value as at times zero may be a valid value (e.g. in the winter). I'd prefer to store nulls, but that's for another day. I don't have a screenshot of this yet as I made the changes at around 1 AM and it looks a bit messy right now.

2) 'State' has two values that I am aware of so far (working and error).  PRTG doesn't do words, just numbers, so I am now converting to integer and reporting these as 0 (working), 1 (error) or 5 (unknown). 5 is completely made up and signifies that the reported state is a value that is unknown to the script.  The unknown value is not stored because this is not a perfect world and perfection is best applied elsewhere.  Moving on, I created a custom lookup that maps these integers to words in the UI... Yes, you guessed it: working; error; and unknown. These are associated with PRTG's up, warning and down. If you are new to PRTG, warning and down are alarms that change the color from green to either yellow or red.  These can optionally be used for notifications.  In its present state a notification would not make sense though as would be generated every day after sunset.  However, if one inverter went into error state during the day, or if all went into error state during the day a notification might be useful. If the panel were covered in snow or leaves, or a random piece of (flying) carpet from the neighbors house you might want to know about it. It would be cool if the neighbor had a flying carpet of course. Ultimate bragging rights to be the owner of that mythical toy.  TBD if I'll build logic in to intelligently alert to error state during the day and if that would be a function of the script or PRTG config.  Here is a screenshot that shows the result of the loopkups.  No code has been posted yet.  Nobody is watching yet, so it's a moot point. :P

What's next: more tweaks based on what I see.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept2.png)

2018/07/26 The POC completed on 7/25 took the serial number provided for any inverter and returned all the values for it. The data returned is interesting.  Now I've added logic to take the serial number provided, determine the device type first (Inverter, Power Meter, Supervisor), and then return the appropriate values for it along with the appropriate labels.  Here is a screenshot showing the single script being used to retrieve values for three device types. The plan is to let this sit, gather data for a couple of days and then work to refine the script. No code has been posted yet.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept1.png)

2018/07/25 Proof of concept has been completed. Working to understand all the different elements next and to formulate an overall monitoring template. Note that because this is an evening project, the returned values are for 'night-time' therefore no energy being produced when the screenshot was created.  :) No code has been posted yet.
![Preview](https://raw.githubusercontent.com/JJWatMyself/SunPower/master/proof-of-concept.png)
