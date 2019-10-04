<div itemscope itemtype="http://developers.google.com/ReferenceObject">
<meta itemprop="name" content="tfa.optimizers.Lookahead" />
<meta itemprop="path" content="Stable" />
<meta itemprop="property" content="iterations"/>
<meta itemprop="property" content="weights"/>
<meta itemprop="property" content="__init__"/>
<meta itemprop="property" content="add_slot"/>
<meta itemprop="property" content="add_weight"/>
<meta itemprop="property" content="apply_gradients"/>
<meta itemprop="property" content="from_config"/>
<meta itemprop="property" content="get_config"/>
<meta itemprop="property" content="get_gradients"/>
<meta itemprop="property" content="get_slot"/>
<meta itemprop="property" content="get_slot_names"/>
<meta itemprop="property" content="get_updates"/>
<meta itemprop="property" content="get_weights"/>
<meta itemprop="property" content="minimize"/>
<meta itemprop="property" content="set_weights"/>
<meta itemprop="property" content="variables"/>
</div>

# tfa.optimizers.Lookahead


<table class="tfo-notebook-buttons tfo-api" align="left">

<td>
  <a target="_blank" href="https://github.com/tensorflow/addons/tree/r0.6/tensorflow_addons/optimizers/lookahead.py#L25-L171">
    <img src="https://www.tensorflow.org/images/GitHub-Mark-32px.png" />
    View source on GitHub
  </a>
</td></table>



## Class `Lookahead`

This class allows to extend optimizers with the lookahead mechanism.



### Aliases:

* Class `tfa.optimizers.lookahead.Lookahead`


<!-- Placeholder for "Used in" -->

The mechanism is proposed by Michael R. Zhang et.al in the paper
[Lookahead Optimizer: k steps forward, 1 step back]
(https://arxiv.org/abs/1907.08610v1). The optimizer iteratively updates two
sets of weights: the search directions for weights are chosen by the inner
optimizer, while the "slow weights" are updated each `k` steps based on the
directions of the "fast weights" and the two sets of weights are
synchronized. This method improves the learning stability and lowers the
variance of its inner optimizer.

#### Example of usage:



```python
opt = tf.keras.optimizers.SGD(learning_rate)
opt = tfa.optimizers.Lookahead(opt)
```

<h2 id="__init__"><code>__init__</code></h2>

<a target="_blank" href="https://github.com/tensorflow/addons/tree/r0.6/tensorflow_addons/optimizers/lookahead.py#L45-L80">View source</a>

``` python
__init__(
    optimizer,
    sync_period=6,
    slow_step_size=0.5,
    name='Lookahead',
    **kwargs
)
```

Wrap optimizer with the lookahead mechanism.


#### Args:


* <b>`optimizer`</b>: The original optimizer that will be used to compute
    and apply the gradients.
* <b>`sync_period`</b>: An integer. The synchronization period of lookahead.
    Enable lookahead mechanism by setting it with a positive value.
* <b>`slow_step_size`</b>: A floating point value.
    The ratio for updating the slow weights.
* <b>`name`</b>: Optional name for the operations created when applying
    gradients. Defaults to "Lookahead".
* <b>`**kwargs`</b>: keyword arguments. Allowed to be {`clipnorm`,
    `clipvalue`, `lr`, `decay`}. `clipnorm` is clip gradients
    by norm; `clipvalue` is clip gradients by value, `decay` is
    included for backward compatibility to allow time inverse
    decay of learning rate. `lr` is included for backward
    compatibility, recommended to use `learning_rate` instead.



## Properties

<h3 id="iterations"><code>iterations</code></h3>

Variable. The number of training steps this Optimizer has run.


<h3 id="weights"><code>weights</code></h3>






## Methods

<h3 id="add_slot"><code>add_slot</code></h3>

``` python
add_slot(
    var,
    slot_name,
    initializer='zeros'
)
```

Add a new slot variable for `var`.


<h3 id="add_weight"><code>add_weight</code></h3>

``` python
add_weight(
    name,
    shape,
    dtype=None,
    initializer='zeros',
    trainable=None,
    synchronization=tf_variables.VariableSynchronization.AUTO,
    aggregation=tf_variables.VariableAggregation.NONE
)
```




<h3 id="apply_gradients"><code>apply_gradients</code></h3>

<a target="_blank" href="https://github.com/tensorflow/addons/tree/r0.6/tensorflow_addons/optimizers/lookahead.py#L93-L95">View source</a>

``` python
apply_gradients(
    grads_and_vars,
    name=None
)
```




<h3 id="from_config"><code>from_config</code></h3>

<a target="_blank" href="https://github.com/tensorflow/addons/tree/r0.6/tensorflow_addons/optimizers/lookahead.py#L165-L171">View source</a>

``` python
@classmethod
from_config(
    cls,
    config,
    custom_objects=None
)
```




<h3 id="get_config"><code>get_config</code></h3>

<a target="_blank" href="https://github.com/tensorflow/addons/tree/r0.6/tensorflow_addons/optimizers/lookahead.py#L156-L163">View source</a>

``` python
get_config()
```




<h3 id="get_gradients"><code>get_gradients</code></h3>

``` python
get_gradients(
    loss,
    params
)
```

Returns gradients of `loss` with respect to `params`.


#### Arguments:


* <b>`loss`</b>: Loss tensor.
* <b>`params`</b>: List of variables.


#### Returns:

List of gradient tensors.



#### Raises:


* <b>`ValueError`</b>: In case any gradient cannot be computed (e.g. if gradient
  function not implemented).

<h3 id="get_slot"><code>get_slot</code></h3>

``` python
get_slot(
    var,
    slot_name
)
```




<h3 id="get_slot_names"><code>get_slot_names</code></h3>

``` python
get_slot_names()
```

A list of names for this optimizer's slots.


<h3 id="get_updates"><code>get_updates</code></h3>

``` python
get_updates(
    loss,
    params
)
```




<h3 id="get_weights"><code>get_weights</code></h3>

``` python
get_weights()
```




<h3 id="minimize"><code>minimize</code></h3>

``` python
minimize(
    loss,
    var_list,
    grad_loss=None,
    name=None
)
```

Minimize `loss` by updating `var_list`.

This method simply computes gradient using `tf.GradientTape` and calls
`apply_gradients()`. If you want to process the gradient before applying
then call `tf.GradientTape` and `apply_gradients()` explicitly instead
of using this function.

#### Args:


* <b>`loss`</b>: A callable taking no arguments which returns the value to minimize.
* <b>`var_list`</b>: list or tuple of `Variable` objects to update to minimize
  `loss`, or a callable returning the list or tuple of `Variable` objects.
  Use callable when the variable list would otherwise be incomplete before
  `minimize` since the variables are created at the first time `loss` is
  called.
* <b>`grad_loss`</b>: Optional. A `Tensor` holding the gradient computed for `loss`.
* <b>`name`</b>: Optional name for the returned operation.


#### Returns:

An Operation that updates the variables in `var_list`.  If `global_step`
was not `None`, that operation also increments `global_step`.



#### Raises:


* <b>`ValueError`</b>: If some of the variables are not `Variable` objects.

<h3 id="set_weights"><code>set_weights</code></h3>

``` python
set_weights(weights)
```




<h3 id="variables"><code>variables</code></h3>

``` python
variables()
```

Returns variables of this Optimizer based on the order created.



