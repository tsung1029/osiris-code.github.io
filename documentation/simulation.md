This section configures global simulation parameters and is optional. 
It accepts the following data: 
\*'''algorithm''', string, default = "standard" 
\*'''random\_seed''', integer, default = 0 
\*'''omega\_p0''', real, default = 0.0 
\*'''n0''', real, default = 0.0 
\*'''gamma''', real, default = 1.0 
\*'''ndump\_prof''', integer, default = 0 
\*'''wall\_clock\_limit''', string, default = "" 
\*'''wall\_clock\_check''', integer, default = -1 
\*'''wall\_clock\_checkpoint''', logical, default = .true. 
'''algorithm''' specifies the algorithm to be used in the simulation. If set to "pgc" the ponderomotive guiding center solver (PGC) algorithm and corresponding particle pusher are used. PGC is currently implemented in 2D geometries only. Please read node configuration and space configuration sections for additional details on PGC usage. If set to "hd-hybrid" the hybrid algorithm is used. 
'''random\_seed''' specifies the seed used by the random number generator. If not set it defaults to 0. If set the random number generator seed used by OSIRIS in each parallel node will be random\_seed + node\_id (or just random\_seed in serial runs). 
'''omega\_p0''' specifies the reference simulation plasma frequency for the simulation in units of \[rad/s\]. The user must set this value when doing simulations that require real (not simulation) units, such as simulations involving ionization or collisions. Setting a reference density, 
'''n0''', overrides this value. See also the '''n0''' option. '''n0''' specifies the reference simulation plasma density for the simulation in units of \[cm^-3\]. Setting this value overrides the 
'''omega\_p0''' option, calculating its value from the supplied value. See also the '''omega\_p0''' option. 

'''gamma''' specifies the relativistic factor of the simulation frame. This factor will transform all zpulses to the boosted frame, and will be used to automatically calculate the 'laserkdx' smooth coefficients. 
'''ndump\_prof''' specifies the frequency for profiling (timing) information dumps. 
'''wall\_clock\_limit''' specifies a time limit to shut down the simulation, in a format "h:m:s". When this wall clock time is reached the simulation will shut down. See also 'wall\_clock\_check'. 
'''wall\_clock\_check''' specifies the frequency at which to check if wall time limit has been reached. See also 'wall\_clock\_limit'. 
'''wall\_clock\_checkpoint''' specifies if the code should dump checkpointing information when stopping due to wall clock limit. 

Here's an example of a simulation section defining a random number seed of 1237 and a reference density of 2.7e17 cm^-3. 

simulation { random\_seed = 1237, n0 = 2.7e17, ! \[cm^-3\] }
