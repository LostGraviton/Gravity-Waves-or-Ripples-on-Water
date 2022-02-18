# Gravity-Waves-or-Ripples-on-Water
import matplotlib.pyplot as plt
import seaborn as sns
import math as mth
import numpy as np
sns.set_style('darkgrid')

def f3(x,k,t):
    return (a/mth.sqrt(4*np.pi)) * (mth.exp((-((k+k0)*a)**2)/4)) * (np.cos(k*x - np.sqrt(abs((g*k) - (G*k**3/p)))*t))  # defined the function to integrate

def f4(x,k,t):
    return (a/mth.sqrt(4*np.pi)) * (mth.exp((-((k-k0)*a)**2)/4)) * (np.cos(k*x - np.sqrt(abs((g*k) - (G*k**3/p)))*t))  # defined the function to integrate


# defined initial conditions

G = 72 #gamma
p = 1000 #rho
a = 6
lam0 = 10.0
k0 = (2* np.pi)/lam0
g = -9.8
kl = -8.0/a
kr = 8.0/a
n = 1000
h = (kr-kl)/n # step size for k


# Now let's integrate
    
for t in range(0,45,15): # time loop
    
    xl = -100.0
    xr = 100.0
    N = 2000
    H = (xr-xl)/N # step size for x
    x = xl
    
    # defined the lists

    X = []
    F = []
    
    for _ in range(N): # x loop
    
        integration1 = f3(x,kl,t) + f3(x,kr,t)

        for i in range(n): # 1st integration loop
        
            j1 = kl + i*h

            if i%2 == 0:
                integration1 = integration1 + 2 * f3(x,j1,t)  # for even terms
  
            else:
                integration1 = integration1 + 4 * f3(x,j1,t)  # for odd terms

        integration1 = integration1 * h/3
    
        integration2 = f4(x,kl,t) + f4(x,kr,t)

        for i in range(n): # 2nd integration loop
        
            j2 = kl + i*h

            if i%2 == 0:
                integration2 = integration2 + 2 * f4(x,j2,t)  # for even terms
            else:
                integration2 = integration2 + 4 * f4(x,j2,t)  # for odd terms

        integration2 = integration2 * h/3   
    
        X.append(x)
        F.append(integration1 + integration2)
        x = x + H
            
    plt.plot(X, F, label = t)    
    plt.legend()
    plt.rcParams["figure.figsize"] = (20, 10)
    plt.xlabel("x")
    plt.ylabel("F")
