Core Concepts
***************

How Clad Works
=================

.. todo:: todo

Forward and Reverse Mode Automatic Differentiation
====================================================

.. todo:: todo

Derived Function Types and Derivative Types
=============================================

.. todo:: todo

Custom Derivatives
====================

.. todo:: todo

Pushforward and Pullback functions
===================================

Pushforward functions 
-------------------------

Pushforward function of a function computes the output variables sensitivities
from the input values and the input variables sensitivities. 
Intuitively, pushforward functions propagates the derivatives forward. Pushforward
functions are constructed by applying the core principles of the forward mode 
automatic differentiation. 

As a user, you need to understand how pushforward function mechanism works so that you
can define custom derivative pushforward functions as require and can thus, unlock full
potential of Clad.

Mathematically, if a function `fn` is represented as\:

.. math::

   fn(u) = sin(u)

then the corresponding pushforward will be defined as follows\:

.. math::

    fn\_pushforward(x, \dot{u}) = cos(u)*\dot{u}

Here, :math:`\dot{u} = \pdv{u}{x}` and :math:`x` is the independent variable.

As a concrete example, pushforward of `std::sin(double)` function will be generated by Clad as follows::

  namespace std {
    double sin_pushforward(double x, double d_x) {
      return ::std::cos(x) * d_x;
    }
  }

In the forward mode automatic differentiation, we need to compute derivative
of each expression in the original source program. Pushforward functions allow to 
effectively compute and process derivatives of function call expressions. For example::

  y = fn(u, v);

In the derived function, this statement will be transformed to::

  _d_y = fn_pushforward(u, v, _d_u, _d_v);
  y = fn(u, v);

Here, note that\:

.. math::

   \_d\_u = \dot{u} = \pdv{u}{x} \\
   \_d\_v = \dot{v} = \pdv{v}{x} \\
   \_d\_y = \dot{y} = \pdv{y}{x} 

Here, :math:`x` is the independent variable.

From here onwards, in the context of a pushforward function or a usual forward
mode derived function, `_d_someVar` will represent :math:`\pdv{someVar}{x}` 
where :math:`x` is the associated independent variable.

Pushforward functions are generated on demand. That is, if Clad needs to 
differentiate a function call expression, 
then only it will generate the pushforward of the corresponding function declaration.

For a function::

  double fn1(float i, double& j, long double k) { }

the prototype of the corresponding pushforward function will be as follows::

  double fn1(float i, double& j, long double k, float _d_i, double& _d_j, long double _d_k);

Please note the following specification of the pushforward functions: 

- Return type of the pushforward function is same as that of the return type of the source function.
- Parameter list of the pushforward function is the parameter list of the original function followed by
  the parameter list that contains derivative of each differentiable parameter of the source function 
  in the original sequence.

All of these specifications must be exactly satisfied when creating a custom 
derivative pushforward function.

Pullback functions
--------------------

.. todo:: todo

Differentiable Class Types
==============================

.. todo:: todo

Numerical Differentiation Core Concepts
==========================================

.. todo:: todo

Error Estimation Core Concepts
================================

.. todo:: todo