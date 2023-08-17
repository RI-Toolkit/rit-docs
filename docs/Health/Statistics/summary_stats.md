# Summary of Statistics
---

### All Survival Stats: `health_stats`

A combination of all the statistics. For static and trend models, if transition probabilities are provided, then
one simulation is run and all the statistics are calculated from that simulation (this is 
to keep results consistent). For frailty model, 'n' number of simulations are performed from which the statistics are calculated. 

The function returns all the information (mean and variance of each statistic) as a dataframe.

**health_stats(model_type, n_states, init_age, init_state, trans_probs, simulated_path, female, year, wave_index, latent, param_file, n = 1000)**

&nbsp;&nbsp; **Parameters:**

&nbsp;&nbsp;&nbsp;&nbsp; model_type : character

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *'S' for static model, 'T' for trend model, 'F' for frailty model*

&nbsp;&nbsp;&nbsp;&nbsp; n_states : numeric

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; *take values 3 or 5, use 3 for 3-state model, and 5 for 5-state model*

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

&nbsp;&nbsp;&nbsp;&nbsp; Mean and variance of all statistics

&nbsp;&nbsp; **Usage:**

```r
# 3 state trend model
trans_probs_3state=get_trans_probs(n_states=3, model_type = 'T', param_file = US_HRS, init_age = 65, female = 0, year = 2022, wave_index = 13, latent = 0)
health_stats(model_type = 'T', n_states=3, init_age=65, init_state=0, trans_probs=trans_probs_3state)
# 3 state frailty model
health_stats(model_type = 'F', n_states=3, init_age=65, init_state=0, trans_probs= NULL , simulated_path = NULL, female = 0, year = 2022, wave_index = 13, latent = 0, param_file = US_HRS)

# 5 state trend model
trans_probs_5state=get_trans_probs(n_states=5, model_type = 'T', param_file = US_HRS_5, init_age = 65, female = 0, year = 2022, wave_index = 13, latent = 0)
health_stats(model_type = 'T', n_states=5, init_age=65, init_state=0, trans_probs=trans_probs_5state)
# 5 state frailty model
health_stats(model_type = 'F', n_states=5, init_age=65, init_state=0, trans_probs= NULL , simulated_path = NULL, female = 0, year = 2022, wave_index = 13, latent = 0, param_file = US_HRS_5)
```