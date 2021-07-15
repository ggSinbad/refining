MCSimulation
================
Alexander Maximiliano Castro Fuentes
01/06/2021

``` r
MCSim <-function(TransitionMatrix , StartingState){
  
  
  
  
  #TransitionMatrix[CurrentState, ] #this will determine the probability of moving to each state
  
  #StateHistory <- matrix(0, 1 , 11) #creates a column vector of length iterations, to store the "history" of the markov chain.
  
  #StateHistory[1,1] <- StartingState
  
  NumberOfStates <- nrow(TransitionMatrix)
  
  CurrentState <- StartingState
  
  
  for (i in 1:10){
    
    Example <- sample(NumberOfStates , size = 1 , prob = TransitionMatrix[CurrentState , ])
    
    print(Example)
    
    CurrentState <- Example
    
  }
  
}
```

``` r
MCSim2 <-function(TransitionMatrix , StartingState){
  
  
  
  
  #TransitionMatrix[CurrentState, ] #this will determine the probability of moving to each state
  
  StateHistory <- matrix(1, 1 , 11) #creates a column vector of length iterations, to store the "history" of the markov chain.
  
  StateHistory[1,1] <- StartingState
  
  NumberOfStates <- nrow(TransitionMatrix)
  
  CurrentState <- StartingState
  
  
  for (i in 1:10){
    
    Example <- sample(NumberOfStates , size = 1 , prob = TransitionMatrix[ StateHistory[i] , ])
    
    print(Example)
    
    StateHistory[i+1] <- Example
    
  }
  
}
```

``` r
MCSim3 <- function(TransitionMatrix, StartingState, length){
  
  
  #TransitionMatrix[CurrentState, ] #this will determine the probability of moving to each state
  
  StateHistory <- matrix(1, 1 , length + 1) #creates a column vector of length iterations, to store the "history" of the markov chain.
  
  StateHistory[1,1] <- StartingState
  
  NumberOfStates <- nrow(TransitionMatrix)
  
  
  
  
  for (i in 1:length){
    
    Example <- sample(NumberOfStates , size = 1 , prob = TransitionMatrix[ StateHistory[i] , ])
    
    print(Example)
    
    StateHistory[i+1] <- Example
    
  }
  
}
```
