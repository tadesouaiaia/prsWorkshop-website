

# Introduction to Python

Python is a useful general purpose programming language.  Here we will go over some 
basic python commands from an interactive shell.  To being open python3 by typing: 

    python3  


## Libraries

Downloaded python libraries can be imported interactively: 

    import matplotlib.pyplot as plt  


## Variables in Python

 You can assign a value or values to any variable you want using the equals sign: 

    X = 5 # X is an integer (5) 
    X = [1,2,3,4,5] # X is a list containing the first five integers 

Functions can also be used: 

    X = range(-10,11,1) # X includes all integers between -10 and 10 counting by 1 

New variables can be declared with lists: 

    Y = [x*x for x in X] # Y is equal to X squared 

A figure can be generated: 

    plt.plot(X,Y)

And viewed: 

    plt.show() 


