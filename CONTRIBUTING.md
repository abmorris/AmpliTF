Some guidelines for code design (still to be discussed). 

   * Follow functional paradigm: code consisting of pure functions and stateless objects. 
   * Try to limit the usage of TensorFlow-specific functions in the places other than amplitf/interface.py to a minimum. The ultimate goal is to have the code that is agnostic to the actual backend, place all TF-specific code to interface.py, and be able to switch backends. 
   * Naming of functions: the functions that return TF graphs should have nouns as the names (e.g. maximum_estimator), while those that internally run the graphs are verbs (e.g. run_minuit). In TF1 terms, it would be the functions which do and do not operate with tf.Session, respectively, but in TF2 this distinction is blurred. 
   * Comments to functions: what convention should we use for self-documentation software? 
   * Conventions for tensor indexing: 
      * The 1st index always corresponds to an "event"
      * The last index corresponds to a variable that characterises the event (e.g. component of a vector or component of the kinematic phase space)
      * There could be inner indices that could be reserved for, e.g. integration over invisible degrees of freedom, for convolution with resolution, etc. Therefore, functions operating with vectors should not assume that the tensof always has two indices. Use ellipsis where needed, e.g. instead of 
```tf.reduce_sum(v[:,0:3], axis=1)``` 
use 
```tf.reduce_sum(v[...,0:3], axis=-1)```
