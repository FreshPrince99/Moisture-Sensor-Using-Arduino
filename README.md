# Moisture-Sensor-Using-Arduino


Urinary incontinence can have a devastating impact on a patient's life, with significant financial implications. Non-surgical treatments such as pelvic floor muscle training and the use of mechanical devices are often the first line of treatment, especially when a patient does not want surgery or when they are deemed unfit for surgery. The mechanical device is inexpensive and does not interfere with future surgical treatment. In view of such a similar situation, we made use of fusion360 technology and Arduino technology to program codes, model outer packaging, design circuits, etc., to make urinary incontinence detection devices and metal detection devices. This is because the device can help patients with early detection and personalised treatment and can prevent the onset of the disease. This paper introduces the design and manufacture of urinary incontinence devices, which can solve the problem of urinary incontinence and improve the quality of life of patients.

In this project, my team and I developed an innovative incontinence sensor designed to detect the presence of urine. Utilizing the DHT11 sensor, we seamlessly programmed and crafted a case that can be comfortably attached to pads using Arduino. The sensor swiftly identifies the presence of urine and triggers a wireless alarm, alerting caregivers promptly. Additionally, we engineered a metal detector to ensure the sensor's functionality and presence while worn by the patient.


Recent technological advances in the detection of urinary events have led to many UI management
systems previously unavailable. Urine detection sensor technology, combined with digital data
processing and transmission technology, has made it possible to make a timely detection of a urination
event and notify the caregiver via an alarm. This wet diaper alarm system (WDAS) helps the caregiver to
change the wet diaper early enough to save the diaper wearer from discomfort and a skin health
problem. A WDAS can also be used to wake a child experiencing enuresis for toilet training, as well as

![image](https://github.com/FreshPrince99/Moisture-Sensor-Using-Arduino/assets/128372678/e5af785c-34d4-4a8e-bea4-4fdd0d75064b)

generate valid urination event data to accurately assess voiding patterns of individuals to establish
personalised urinary continence care plans. This diagram shows how the device works.
The initial wearable device designed for managing urinary incontinence, the DFree, employs ultrasound
technology to continuously monitor bladder volume. The sensor must be positioned approximately 0.5
inches above the pubic bone for accurate bladder detection. Utilising safe and non-invasive ultrasound
technology, the DFree device is akin to the ultrasound technology commonly employed in prenatal care
and various consumer products like toothbrushes, ensuring its safety for users. Through connectivity
with smart devices, the DFree system transmits alerts to a user's smartphone or tablet, notifying them
when it is advisable to visit the restroom. Users can customise notification thresholds to specify when
alerts are received, allowing for sufficient time to respond to the notification.
In our experiment, we put a temperature and humidity sensor in the middle of the diaper, just like the
picture above, to absorb moisture and to detect it and send an alarm. Following a sequence of
experimental procedures, a total of four laboratory sessions were dedicated to conducting various tests.
Subsequently, the testing apparatus and the incontinence device were successfully fabricated and
demonstrated operational functionality.

**More about the code**

For the part of the circuit that is used to detect the change in inductance, only two pins, A0 and A1, are
needed. Pin A0 is referred to using the variable pin_pulse and is used to apply pulses to the search coil.
Pin A1, or pin_cap, is used as an output pin in order to drain the charge stored in the capacitor and
switched to an input in order to measure the charge stored in the capacitor.
In the main loop() function, the variable ‘sum’ is created and set to 0. This is the local sum and applies
only the measurements taken in this iteration of the loop() function. This is then followed by the for loop
that applies the pulses and measures the charge on the capacitor. This for loop will be referred to as the
‘measurement loop’ in this report.

At the beginning of the measurement loop, pin_cap is utilised as an output and set to LOW for 20ms,
before being switched back into an input, ready for the measurements. Then, the pulses are applied,
followed by the measurement of the charge. The value for the delays was initially
3ms, but the alternate value of 2ms
was decided on after a slightly
improved range was observed.
The measured value is then added to
the local sum. The program also keeps
track of the minimum and maximum
values, which will later be subtracted
from the sum in order to account for
positive and negative spikes in the
readings. For one iteration of the loop() function, 258 measurements are taken, and after the min/max
values are removed, 256 readings remain


**Processing the measurements**

In this part of the code, two more variables are used to store sums. As previously mentioned, ‘sum’ is the
local sum of the 256 measurements taken in one iteration of the main loop() function. The variable
‘sumsum’ is the sum of the local sum over 64 iterations of the main loop(). The third variable ‘avgsum’ is
calculated using the formula “avgsum=(sumsum+32)>>6”. So, ‘avgsum’ is the long-running average of
‘sum’ and is used to calculate the difference in the general trend and the latest measurement, which can
be used to tell whether the measured values have stayed the same, or changed between the consecutive
iterations of loop(). If sumsum is equal to zero, then
it is set to the value of sum times
5
64. This multiplication is calculated by left shifting the value of sum by 6, since bitshift operations are
more efficient than multiplication.

In the latter two lines, the value of avgsum and the difference between the local sum and the
long-running avgsum are calculated.
This bit of code decides whether to adjust the value of sumsum or not. If the variable ‘diff’ is small, then
the local sum has not deviated too much from the long-running avgsum, so the value of sumsum and
avgsum is adjusted to account for the small change. However, if there is a significant difference between
the local sum and the long-running avgsum, then the value of sumsum is kept the same, and instead a
variable named ‘skip’ is
incremented. When ‘skip’
reaches 64, the value of
sumsum is reset using the
initial formula of
‘sumsum=sum<<6;’ and ‘skip’
is also reset to 0.
The variable ‘flash_period’ is
the time, in milliseconds,
between LED flashes. If diff is
0, then there is no change in inductance,
therefore the LEDs don’t need to be on. The first
conditional statement is used for this purpose,
and it also avoids dividing by zero. The else
statement calculates a value for flash_period in the case that diff is not zero. The inverse relationship
between flash_period and diff means that a greater change in inductance will result in a shorter
flash_period, and more frequent LED flashes.

**LED and Buzzer Outputs**
This is the part of the code that decides
Which LED is flashed or which buzzer is sounded.
The variable ledstat determines which LED is turned
on and what frequency the buzzer sound is, with 1
being pin_LED1 and 500 Hz, 2 being pin_LED2 and
2000 Hz, and 0 being neither LED and no sound. The
pin_LED1 represents an increase in inductance, and
pin_LED2 represents a decrease.
The first if statement checks if less than 10ms has
passed since the last flash, and if so, it assigns the
appropriate value to ledstat.
6

The second if statement checks If the flash_period, calculated in the previous snippet of code, has
passed since the prev_flash. If so, the appropriate value is assigned to ledstat and the current time is
assigned to prev_flash, this marks the start of a distinct flashing of the LEDs.
The third if statement turns off both LEDs and the buzzer if the flash_period is greater than 1000. The
greater the flash_period is, the smaller the diff will be, due to the two being inversely proportional. This
selection adds a cutoff point where the LEDs are switched off in order to prevent small fluctuations in
the inductance from showing up in the output.
These three separate if statements could have easily been replaced with an if-else if structure, however
the code efficiency was not a problem during the development.


