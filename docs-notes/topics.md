topics.md
======

Discussed usability issues.  
------

1. Their choice of pseudo-random number generator (PRNG) relies on generating a seed using a timestamp.  
	- Clearly problematic - can't reproduced results, but simulations run at roughly the same time on separate days are close to identical.
	- FIX: provide an ability to specify the seed, use legitimate PRNG.  Investigate if "how random" changes the simulation in any way. 

2. Simulating effective population size, scaled mutation rate, hyper-parameter theta (Scaled Mutation Rate).
	- Behind the scenes, it's not exactly doing what it's expected to based on the mathematics.  
	- Need to identify how the scaled mutation rate parameter may be specified for all use-cases. 
	- Have to figure out how they're determining scale factors for using HAPMAP data. 

3. Outputs
	- They'll provide a whole litany of output for the simulation, but it isn't fully clear what all information is truly captured/reported
		+ (in edge cases in code, includes undocumented reporting capability)
	- Needle in a haystack - what is it we actually need again? 
		+ RAREsim used to address this, figure it out? 

4. Simulated sample size - Cases & Controls
	- Need to provide ability to simulate a number of individuals as "All Cases"?
