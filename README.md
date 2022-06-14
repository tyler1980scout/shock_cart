# shock_cart
Dyno shock cart code for raspberry pi controller
#!/usr/bin/python3

import piplates.RELAYplate2 as r
import piplates.DAQCplate as d 
import time, sys, traceback
import datetime

print("Hello! Welcome to: THE SHOCK CART!!! *start WWE entry music* \n")

#time.sleep(3)

cold_temp_dur = input("How many seconds do you want cold side running? \n")
hot_temp_dur = input("How many seconds do you want hot side running? \n")
#cycle_time = input("How long in seconds do you want cycle to run?")



#countdown class
def countdown(h,m,s):
    
    #calculate total number of seconds
    total_seconds = h*3600 + m*60 + s
    
    #while loop that checks if total_seconds reaches zero
    #if not zero, reduce total time by one second
    while total_seconds >0:
        
        #timer represents time left on countdown
        timer = datatime.timedelta(seconds = total_seconds)
        
        #prints the time left on the timer
        print(timer, end="\r")
        
        #delays program one second
        time.sleep(1)
        
        #reduces total time by one second
        total_seconds -= 1
        
    print("Cycle time met, system shutting down")

h = input("Enter cycle time in hours: ")
m = input("Enter cycle time in minutes: ")
s = input("Enter cycle time in seconds: ")





try:
    r.RESET(1)
    #print(r.getID(1)) # relay board at address 1 meaning the addr0 header is removed only
    start_temp_c = round(d.getTEMP(0,0,'c'),2)
    
    d.getID(0) #daq plate at address 0, all headers are filled
    d.getTEMP(0,0,'c') # (address 0,input 0, 'f'=farenheit 'c'=celsius)
    print(f"Starting temp is: {start_temp_c}")
    
             
   

    while True:
        
        #below relay setting are for cold temp engage duration ##all solenoids should be N/O(hopefully)! 
        r.relayOFF(1,1)
        print(f'Cold side on for {cold_temp_dur} seconds')
        
        r.relayON(1,2)
       
        r.relayOFF(1,3)
        
        r.relayON(1,4)
        
        r.relayOFF(1,5)
        
        r.relayON(1,6)
        time.sleep(int(cold_temp_dur))

        
        
        #below relay setting are for hot temp engage duration ##all solenoids should be N/O(hopefully)! 
        r.relayON(1,1)
        print(f'Hot side on for {hot_temp_dur} seconds')
        
        r.relayOFF(1,2)
       
        r.relayON(1,3)
        
        r.relayOFF(1,4)
    
        r.relayON(1,5)
        
        r.relayOFF(1,6)
        time.sleep(int(hot_temp_dur))
        
        
        current_temp_c = round(d.getTEMP(0,0,'c'),2)
        print(f"Current temp of test unit is: {current_temp_c}")
        
        #def countdown is used to determine cycle length
        def countdown(h,m,s):
            
            #calculate total number of seconds
            total_seconds = h*3600 + m*60 + s
            
            #while loop that checks if total_seconds reaches zero
            #if not zero, reduce total time by one second
            while total_seconds >0:
                
                #timer represents time left on countdown
                timer = datatime.timedelta(seconds = total_seconds)
                
                #prints the time left on the timer
                print(timer, end="\r")
                
                #delays program one second
                time.sleep(1)
                
                #reduces total time by one second
                total_seconds -= 1
                
            print("Cycle time met, system shutting down")

        #h = input("Enter cycle time in hours: ")
        #m = input("Enter cycle time in minutes: ")
        #s = input("Enter cycle time in seconds: ")

        
        
        
        if countdown == 0:
            r.RESET(1)
            break
        
        
except KeyboardInterrupt:
    print('\nQUITTING....\n')
    r.RESET(1)
    sys.exit()
except:
    print(traceback.format_exc())
    #hi

