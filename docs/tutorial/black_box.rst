Black Box Optimization Problem
==============================

Overview
-------------

::

  minimize  simulator(a, b)
  s.t.      0 <= a <= 1, a is integer
            1 <= b <= 2, b is continuous


This optimization problem is a kind of the *mixed integer constrained Black Box Optimization programming*.
This problem can be formulated using `flopt` as follows,

.. code-block:: python

  from flopt import Variable, CustomExpression, Problem, Solver

  # variables
  a = Variable(name='a', lowBound=0, upBound=1, cat='Integer')
  b = Variable(name='b', lowBound=1, upBound=2, cat='Continuous')

  # objective function
  def simulator(a, b):
      return simulator_func(a, b)

  custom_obj = CustomExpression(func=user_func, variables=[a, b])

  # problem
  prob = Problem(name='CustomExpression')
  prob += custom_obj

  # solver
  solver = Solver(algo='RandomSearch')
  solver.setParams(timelimit=60)
  prob.solve(solver, msg=True)


  # get best solution
  print('obj value', prob.getObjectiveValue())
  print('a', a.value())
  print('b', b.value())


CustomExpression
----------------

We can create complex or black-box functions using :doc:`../api_reference/CustomExpression`.
We input two items to CustomExpression.
One is the python function,
and another is the list or tuple of variables in the same order as the arguments in the function.

.. code-block:: python

  def simulator(a, b):
      return simulator_func(a, b)

  custom_obj = CustomExpression(func=user_func, variables=[a, b])


When the function has a list of variables as arguments, we have the following.

.. code-block:: python

  def obj(x):
      return sin(x[0])+cos(x[1])

  x0 = Variable(name='x0', 1, 2, 'Continuous')
  x1 = Variable(name='x1', 1, 2, 'Continuous')
  x = [x0, x1]
  custom_obj = CustomExpression(func=obj, variables=[x])
