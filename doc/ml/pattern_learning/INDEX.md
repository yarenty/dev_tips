# Pattern learning


## Papers
### Pattern-based learning and control of nonlinear pure-feedback systems with prescribed performance

Fukai Zhang, Weiming Wu, Cong Wang

This article presents a novel pattern-based intelligent control scheme with prescribed performance (PP) for uncertain pure-feedback systems operating in multiple control situations (patterns). Based on PP, an observer-based adaptive neural network (NN) control approach, which not only achieves system stability and prescribed tracking control performance but also realizes accurate identification/learning of the unknown closed-loop dynamics via deterministic learning and uses only one NN unit, is proposed. Subsequently, the knowledge learned is utilized to construct high-performance candidate controllers for each control situation. Based on the transformed system and observer technique, accurate classification of the nth order systems under different control situations is achieved by requiring only one set of dynamic estimators, thereby significantly reducing the complexity of pattern recognition. Thus, sudden changes in the control situation can be rapidly recognized based on the minimum residual principle, with which the correct candidate controller is selected to achieve superior control performance. The simulation results verify the efficacy of the proposed scheme.

https://link.springer.com/article/10.1007/s11432-021-3434-9

https://www.sciencedirect.com/science/article/abs/pii/S0016003219300432

[paper](res/112202.pdf)

### Neural learning control of pure-feedback nonlinear systems

MinWang · Cong Wang

Received: 4 June 2014 / Accepted: 26 November 2014 / Published online: 11 December 2014

This paper is concerned with the problem
of learning control for a class of pure-feedback systems
with unknown non-affine terms in dynamical environments.
The implicit function theorem and the mean
value theorem are firstly used to transform the closedloop
system into a semi-affine form. By combining the
quadratic-type Lyapunov function and the appropriate
inequality technology, a concise adaptive neural control
scheme is developed to simplify the system stability
analysis and guarantee the convergence of the tracking
error in a finite time. After the stable control design, we
decompose the closed-loop system into a series of linear
time-varying perturbed subsystems with the help
of the linear state transformation. Using a recursive
design, the partial persistent excitation (PE) condition
for the radial basis function neural network is satisfied
during tracking control to a recurrent reference
trajectory. Under the PE condition, accurate approximations
of the implicit desired control dynamics are
recursively achieved in a local region along recurrent
orbits of closed-loop signals. Subsequently, a neural
learning control method which effectively utilizes the
learned knowledge without re-adapting to the unknown
system dynamics is proposed to achieve the closed-
loop stability and improved control performance. Simulation
studies are performed to demonstrate that the
proposed learning control scheme not only can approximate
accurately the implicit desired control dynamics,
but also can reuse the learned knowledge to achieve
the better control performance with the faster tracking
convergence rate and the smaller tracking error.
Keywords Adaptive neural control ·
Dynamic learning · Pure-feedback systems ·
Persistent excitation · Exponential stability

[paper](res/2015ND.pdf)


### Event-Triggered Neural Sliding Mode Guaranteed Performance Control

Guofeng Xia, Liwei Yang and Fenghong Xiang 

Faculty of Information Engineering and Automation, Kunming University of Science and Technology,
Kunming 650500, China


 To solve the trajectory tracking control problem for a class of nonlinear systems with
  time-varying parameter uncertainties and unknown control directions, this paper proposed a neural
  sliding mode control strategy with prescribed performance against event-triggered disturbance. First,
  an enhanced finite-time prescribed performance function and a compensation term containing the
  Hyperbolic Tangent function are introduced to design a non-singular fast terminal sliding mode
  (NFTSM) surface to eliminate the singularity in the terminal sliding mode control and speed up the
  convergence in the balanced unit-loop neighborhood. This sliding surface guarantees arbitrarily
  small overshoot and fast convergence speed even when triggering mistakes. Meanwhile, we utilize
  the Nussbaum gain function to solve the problem of unknown control directions and unknown
  time-varying parameters and design a self-recurrent wavelet neural network (SRWNN) to handle the
  uncertainty terms in the system. In addition, we use a non-periodic relative threshold event-triggered
  mechanism to design a new trajectory tracking control law so that the conventional time-triggered
  mechanism has overcome a significant resource consumption problem. Finally, we proved that all
  the closed-loop signals are eventually uniformly bounded according to the stability analysis theory,
  and the Zeno phenomenon can be eliminated. The method in this paper has a better tracking effect
  and faster response and can obtain better control performance with lower control energy than the
  traditional NFTSM method, which is verified in inverted pendulum and ball and plate system.


  Keywords: unknown control direction; finite-time prescribed performance control; self-recurrent
  wavelet neural network (SRWNN); non-singular fast terminal sliding mode (NFTSM); Nussbaum
  gain function; event-triggered control (ETC)

[paper](res/processes-10-01742-v2.pdf)






