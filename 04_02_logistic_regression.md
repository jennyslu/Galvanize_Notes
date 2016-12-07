# Morning Lecture

## Logistic Regression Pt. 1

The equation we get: P(X) = h(β + βx + ...) draws a __hyperplane__ that divides the data into two classes: positive and negative.

# Afternoon Lecture

## Logistic Regression Pt. 2

__We want to have a hyperplane where all the 1s are on one side and all the 0s are on the other. Logistic regression will not find the *best* hyperplane; that's SVM.__

The βs are the extent that every x contributes to the log-odds. Then you can go back and see how the log-odds contributes to probability itself. Unfortunately it is difficult to convert between log odds and probability. _Is there any physical meaning to β<sub>0</sub>?_ It is the distance between hyperplane and 0. It can also be thought of as the default probability. For example, if it's logistic regression for surviving the Titantic β<sub>0</sub> is the BASE probability of surviving (given all x's are 0). 

We do not actually get β; we just get the direction in which we have to move to make β better and usually you also apply some sort of learning??
