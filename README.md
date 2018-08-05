# SunPower
This project is for monitoring SunPower solar using PRTG with Perl. Notes in reverse chronology are below

2018/08/05 Ohhh, I have some knew stuff that I have been working on and ready to share an udate.  :)

I have created five new fields which are calculations from realtime values combined with values that are derived from the manufacturer spec sheet. The new fields are:

    p_mpptcont_output_power (%)
  
    p_3phcont_output_power (%)
  
    actual_inv_eff (%)
  
    rated_inv_eff (%) (not output directly by the script)
  
    delta_inv_eff (%)
 

To understand what these mean, consider the following details.

Stated specification:

    •	Pnom 335 W
  
    •	AC Max. Cont. Output Power 320 W

We can compare p_mpptsum_kw against ‘Pnom’ and p_3phsum_kw against ‘AC Mac. Cont. Output Power’. The resulting calculation is p_mpptcont_output_power (%) and p_3phcont_output_power (%).  These two fields provide a more intuitive evaluation of power production vs. maximum possible. On a sunny day we want to see these at 100%. If lower than 100%, then this might be an indication of a problem with the panel or some other environmental factor, e.g. dirt on panel, sub-optimal angle, shade, or simply not sunny enough.

The next area we can draw a conclusion is DC/AC CEC Conversion Efficiency. i.e. power in vs power out for the inverter.

Stated specification:

     •	DC/AC CEC Conversion Efficiency 96%

This is a simple calculation. 

    Pnom ÷ AC Max. Cont. Output Power = DC/AC CEC Conversion Efficiency

Therefore, we can compare p_3phsum_kw to p_mpptsum_kw and calcualte the actual_inv_eff (%)

Before you look at this and assume that 96% would be the target value for actual_inv_eff (%), how about we use a more accurate ‘DC/AC CEC Conversion Efficiency’ value. We’ll call this rated_inv_eff (%).

    335.00 ÷ 320.00 = 0.955224 (𝑜𝑟 95.5224%)

Technically the spec sheet isn’t wrong, they have chosen to display an integer and rounded up: 96%. But the rating is less than 96%, so increasing accuracy on this can help us improve decisions we might make when analyzing the inverter efficiency. Therefore we want actual_inv_eff (%) to be 95.5224% or greater.

So for our fith field delta_inv_eff (%) we will analyze the inverter efficiency in a more intuitive fashion by comparing actual_inv_eff (%) to rated_inv_eff (%). This tells us how close (delta) to our super accurate version of ‘DC/AC CEC Conversion Efficiency’ the inverter is performing.  A positive value means the performance has exceeded (hurray!) the spec. A negative value means that the inverter is not performing to the stated spec (boooo!).  Observing these calculations for just one day I have already been able to conclude that when the power is less than 10-15%, the inverter under performs. At dawn and dusk or on a cloudy day if power is less than 10-15%, there is more power loss at the inverter. i.e. actual_inv_eff (%) < 95.5224%. The efficiency is as low as 65%. Who would have known?  At that power level though, who cares what the efficiency is.


So with all this new derived data, we can use the additional five new fields and make some decisions:

Scenario 1:

    If it’s a beautiful sunny day

    And p_mpptcont_output_power (%) and p_3phcont_output_power (%) < 100%
  
    Then the panel may be in the shade, dirty, sub-optimal angle or have a problem ☹

Scenario 2:

    If it’s a beautiful sunny day

    And p_mpptcont_output_power (%) = 100%
  
    And delta_inv_eff (%) < 0% (i.e. p_mpptcont_output_power (%) < 100%)
  
    Then the inverter likely has a problem ☹

So that's it for the changes. May you warrenty claims be plentiful!


Beyond these five new fields, let’s have some geeky fun and discuss the specs a little further.   Imagine we are comparing spec sheets from different manufacturers that have the same Pnom and ‘AC Max. Cont. Output Power’ but the 'DC/AC CEC Conversion Efficiency' differs. Here are two stated specification examples:

Manufacturer 1:

    •	Pnom 335 W
  
    •	AC Max. Cont. Output Power 320 W
  
    •	DC/AC CEC Conversion Efficiency 96%

Manufacturer 2:

    •	Pnom 335 W
  
    •	AC Max. Cont. Output Power 320 W
  
    •	DC/AC CEC Conversion Efficiency 95%

Manufacturer 1 is not better than manufacturer 2. They are in fact the identical spec! Here’s why.

Let’s assume each manufacturer’s spec could mean this:

    •	Pnom is between 334.5W and 335.49W
  
    •	AC Max. Cont. Output Power is between 319.5W and 320.49W

We can calculate all possible combinations of efficiency in an Excel table and see that maximum efficiency is somewhere in the region of 95.2338% to 95.8117%.  Here are two of the corners of that table:

    335.49 ÷319.50 =0.952338 (𝑜𝑟 95.2338%)
  
    334.5.0 ÷320.49 =0.958117 (𝑜𝑟 95.8117%)

Manufacturer 1 and 2 can pick a value anywhere in this table and be telling the truth.  Or they can simply choose to round up or round down, which is technically the truth also.  Some manufacturers may even use this to their advantage and increase the DC/AC CEC Conversion Efficiency’ (marketing B.S.). Make sense?

Oh and no code has been posted yet. :D

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
