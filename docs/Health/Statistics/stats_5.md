# 5-State Model

With the use of simulated paths, a lot of mortality statistics can be generated 
to analyse or simply gain insight into the survival characteristics of certain individuals. 

For the 5 state model, we use the simulated paths as inputs to calculate:

* the average time of entering a certain state (such as the D state: Healthy and functionally disabled)

* the total time spent in each state 

As results are stochastic (statistics are derived from random simulated lifetimes), we can 
also study the variance of these statistics. This will be beneficial in quantifying the risk, 
allowing for more robust pricing methods. 

---

### Average First Time Entering State

**health5_first_time_stats(model_type, state, init_age, init_state, trans_probs, simulated_path, female, year, wave_index, latent, param_file, n = 1000)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; model_type : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; state : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting which state we are entering or leaving*

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0 for first time leaving H state, only used when initial state is 0

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1 for first time entering M state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2 for first time entering D state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3 for first time entering MD state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -1 for first time entering the dead state

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting initial age of indiviudal*

&nbsp;&nbsp;&nbsp;&nbsp; init_state : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial state of individual: 0 for healthy, 1 for disabled*

&nbsp;&nbsp;&nbsp;&nbsp; trans_probs : list

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *list of transition probability matrices, only needed for static and trend models.*

&nbsp;&nbsp;&nbsp;&nbsp; simulated_path : matrix

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *matrix containing life path simulations, only needed for static and trend models.*

&nbsp;&nbsp;&nbsp;&nbsp; female : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *0 for male, 1 for female, compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; year : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting current year, compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; wave_index : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer for the wave index = (interview year - 1998)/2 + 1, required in 5-state model and ignored in 3-state model*

&nbsp;&nbsp;&nbsp;&nbsp; latent : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial value of latent factor, normally take the value 0*

&nbsp;&nbsp;&nbsp;&nbsp; param_file : character OR dataframe/tibble

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *File path, or dataframe/tibble of parameters (generally, use US_HRS or china_CLHLS), compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; n : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting number of unique latent factor simulations*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; Column vector of first time entering the specified state

&nbsp;&nbsp; **Usage:**

```r
# trend model
trans_probs <- get_trans_probs(n_states=5, model_type='T', param_file=US_HRS_5, init_age=65, female=0, year = 2022, wave_index = 13, latent = 0)
simulated_path <- simulate_health_state_paths(trans_probs, init_age=65, init_state = 0, cohort = 10000)

# time until entering M state, ill health but not functionally disabled
time_to_M <- health5_first_time_stats(model_type = 'T', state = 1, init_age = 65, init_state = 0, trans_probs)

# average initial time of entering state 1
print(mean(time_to_M, na.rm = TRUE))

# frailty model
time_to_M <- health5_first_time_stats(model_type = 'F', state = 1, init_age = 65, init_state = 0, female = 0, year = 2022, wave_index = 13, latent = 0, param_file = US_HRS_5)
print(mean(time_to_M, na.rm = TRUE))
```

---

### Total Time Spent in State

**health5_total_time_stats(simulated_path, state)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; model_type : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; state : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer denoting which state we are entering or leaving*

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 0 for total time in H state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1 for total time in M state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 2 for total time in D state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 3 for total time in MD state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; -1 for total time in dead state

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4 for total time alive or not in dead state

&nbsp;&nbsp;&nbsp;&nbsp; init_age : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting initial age of indiviudal*

&nbsp;&nbsp;&nbsp;&nbsp; init_state : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial state of individual: 0 for healthy, 1 for disabled*

&nbsp;&nbsp;&nbsp;&nbsp; trans_probs : list

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *list of transition probability matrices, only needed for static and trend models.*

&nbsp;&nbsp;&nbsp;&nbsp; simulated_path : matrix

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *matrix containing life path simulations, only needed for static and trend models.*

&nbsp;&nbsp;&nbsp;&nbsp; female : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *0 for male, 1 for female, compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; year : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting current year, compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; wave_index : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *integer for the wave index = (interview year - 1998)/2 + 1, required in 5-state model and ignored in 3-state model*

&nbsp;&nbsp;&nbsp;&nbsp; latent : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *initial value of latent factor, normally take the value 0*

&nbsp;&nbsp;&nbsp;&nbsp; param_file : character OR dataframe/tibble

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *File path, or dataframe/tibble of parameters (generally, use US_HRS or china_CLHLS), compulsory variable for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; n : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *numeric denoting number of unique latent factor simulations*

&nbsp;&nbsp; **Returns:**

&nbsp;&nbsp;&nbsp;&nbsp; Column vector of total times spent in specified state

&nbsp;&nbsp; **Usage:**

```r
# trend model
trans_probs <- get_trans_probs(n_states=5, model_type='T', param_file=US_HRS_5, init_age=65, female=0, year = 2022, wave_index = 13, latent = 0)
simulated_path <- simulate_health_state_paths(trans_probs, init_age=65, init_state = 0, cohort = 10000)

# time until entering M state, ill health but not functionally disabled
total_time_MD <- health5_first_time_stats(model_type = 'T', state = 3, init_age = 65, init_state = 0, trans_probs)

# average initial time of entering state 1
print(mean(total_time_MD, na.rm = TRUE))

# frailty model
total_time_MD <- health5_first_time_stats(model_type = 'F', state = 3, init_age = 65, init_state = 0, female = 0, year = 2022, wave_index = 13, latent = 0, param_file = US_HRS_5)
print(mean(total_time_MD, na.rm = TRUE))
```

