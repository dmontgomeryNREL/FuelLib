FuelLib
=======

The **Fuel Library** for advanced research on evaporation **(FuelLib)** utilizes
the group contribution method (GCM), as developed by Constantinou and 
Gani\ :footcite:p:`constantinou_new_1994,constantinou_estimation_1995` in the mid-1990s, 
to provide a systematic approach for estimating the thermodynamic properties of
pure organic compounds. The GCM decomposes molecules into structural groups, 
each contributing to a target property based on predefined group values. 
By summing these contributions, the GCM accurately predicts essential properties, 
including the acentric factor, normal boiling point, liquid molar volume at standard conditions 
(298 K) and more. This predictive capability is particularly useful for complex 
mixtures such as sustainable aviation fuels (SAFs), where experimental thermodynamic data 
is limited. `FuelLib` provides SAF developers with a means to estimate 
these critical properties without extensive physical testing, thereby aiding in 
the identification of promising fuel compositions before committing to large-scale production.

`FuelLib` was developed with SAF research in mind. It builds on 
`Pavan B. Govindaraju's Matlab implementation <https://github.com/gpavanb-old/GroupContribution>`_, 
and includes gas chromatography data (GC x GC) for various jet fuels from the Air Force Research Laboratory\ :footcite:p:`edwards_jet_2020`.
Additionally, `FuelLib` includes correlations for the thermodynamic properties of 
mixture such as density, viscosity and vapor pressure. The Section :ref:`tab-GCM-properties` 
outlines the properties for the *i-th* compound in a mixture, which depends on 
the *k-th* first-order and *j-th* second-order group contributions.

.. _tab-GCM-properties:

Table of GCM properties
-----------------------

.. table:: GCM properties of the *i-th* component in a mixture. The subscript *stp* denotes a standard pressure assumption.
   :widths: auto
   :align: center

   ====================================  =====================  ===========================================  ====================  ===========================================================
   Property                              Units                  Group Contributions                          Units                 Description
   ====================================  =====================  ===========================================  ====================  ===========================================================
   :math:`M_{w,i}`                       kg/mol                 :math:`m_{w1k}`                              g/mol                 Molecular weight.
   :math:`T_{c,i}`                       K                      :math:`t_{c1k}`, :math:`t_{c2j}`             1                     Critical temperature\ :footcite:p:`constantinou_new_1994`.
   :math:`p_{c,i}`                       Pa                     :math:`p_{c1k}`, :math:`p_{c2j}`             bar\ :sup:`-0.5`      Critical pressure\ :footcite:p:`constantinou_new_1994`.
   :math:`V_{c,i}`                       m\ :sup:`3`\ /mol      :math:`v_{c1k}`, :math:`v_{c2j}`             m\ :sup:`3`\ /kmol    Critical volume\ :footcite:p:`constantinou_new_1994`.
   :math:`T_{b,i}`                       K                      :math:`t_{b1k}`, :math:`t_{b2j}`             1                     Normal boiling point\ :footcite:p:`constantinou_new_1994`.
   :math:`T_{m,i}`                       K                      :math:`t_{m1k}`, :math:`t_{m2j}`             1                     Normal melting point\ :footcite:p:`constantinou_new_1994`.
   :math:`\Delta H_{f,i}`                J/mol                  :math:`h_{f1k}`, :math:`h_{f2j}`             kJ/mol                Enthalpy of formation at 298 K\ :footcite:p:`constantinou_new_1994`.
   :math:`\Delta G_{f,i}`                J/mol                  :math:`g_{f1k}`, :math:`g_{f2j}`             kJ/mol                Standard Gibbs free energy at 298 K\ :footcite:p:`constantinou_new_1994`.
   :math:`\Delta H_{v,\textit{stp},i}`   J/mol                  :math:`h_{v1k}`, :math:`h_{v2j}`             kJ/mol                Enthalpy of vaporization at 298 K\ :footcite:p:`constantinou_new_1994`.
   :math:`\omega_i`                      1                      :math:`\omega_{1k}`, :math:`\omega_{2j}`     1                     Acentric factor\ :footcite:p:`constantinou_estimation_1995`.
   :math:`V_{m,\textit{stp},i}`          m\ :sup:`3`\ /mol      :math:`v_{m1k}`, :math:`v_{m2j}`             m\ :sup:`3`\ /kmol    Liquid molar volume at 298 K\ :footcite:p:`constantinou_estimation_1995`. 
   :math:`C_{p,\textit{stp},i}`          J/mol/K                :math:`C_{pA1_k}`, :math:`C_{pA2_k}`,...     J/mol/K               Specific heat capacity\ :footcite:p:`nielsen_molecular_1998,poling_properties_2001`.
   ====================================  =====================  ===========================================  ====================  ===========================================================

.. _eq-GCM-properties:

Equations for GCM properties
----------------------------

The properties of each compound in a mixture can be calculated as the sum of contributions from the first- and second-order groups that make up the compound. For a given mixture, let :math:`\mathbf{N}` be an :math:`N_c \times N_{g_1}` matrix that represents the number of first-order groups in each compound, where $N_c$ is the number of compounds in the mixture and :math:`N_{g_1}` is the total number of first-order groups as defined by Constantinou and Gani\ :footcite:p:`constantinou_new_1994,constantinou_estimation_1995`.  Similarly, let :math:`\mathbf{M}` be an :math:`N_c \times N_{g_2}` matrix that specifies the number of second-order groups in each compound, where :math:`N_{g_2}` is the total number of second-order groups. The total number of groups :math:`N_g = N_{g_1} + N_{g_2} = 121`. Define a parameter :math:`W` such that :math:`W = 0` performs a first-order group only calculation, while :math:`W = 1` includes second-order groups. The GCM properties for the *i-th* compound in the mixture are calculated as follows\ :footcite:p:`constantinou_new_1994,constantinou_estimation_1995,poling_properties_2001`:

.. math::

   \begin{align*}
    M_{w,i} &= \bigg[\sum_{k = 1}^{N_{g_1}}\mathbf{N}_{ik}m_{w1k} \bigg] \times 10^{-3}, \\
    T_{c,i} &= 181.28 \ln  \bigg[ \sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} t_{c1k} + W \sum_{j=1}^{N_{g_2}}         \mathbf{M}_{ij} t_{c2j} \bigg],\\
    p_{c,i} &= \Bigg( \bigg[  \sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} p_{c1k} + W \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij}     p_{c2j} + 0.10022\bigg]^{-2}  + 1.3705\Bigg)\times 10^{5}, \label{eq:gcm-pc}\\
    V_{c,i} &= \Bigg( \bigg[ \sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} v_{c1k} + W \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij}      v_{c2j} \bigg] -0.00435 \Bigg)\times 10^{-3}, \\
    T_{b,i} &= 204.359 \ln  \bigg[ \sum_{k = 1}^{N_{g_1}} \mathbf{N}_{ik} t_{b1k} + W \sum_{j=1}^{N_{g_2}}      \mathbf{M}_{ij} t_{b2j}\bigg],\\
    T_{m,i} &= 102.425 \ln  \bigg[ \sum_{k = 1}^{N_{g_1}} \mathbf{N}_{ik} t_{m1k} + W \sum_{j=1}^{N_{g_2}}      \mathbf{M}_{ij} t_{m2j}\bigg],\\
    \Delta H_{f,i} &= \Bigg( \bigg[ \sum_{k = 1}^{N_{g_1}} \mathbf{N}_{ik} h_{f1k} + W \sum_{j=1}^{N_{g_2}}     \mathbf{M}_{ij} h_{f2j} \bigg] + 10.835\Bigg) \times 10^3,\\
    \Delta G_{f,i} &= \Bigg( \bigg[ \sum_{k = 1}^{N_{g_1}} \mathbf{N}_{ik} g_{f1k} + W \sum_{j=1}^{N_{g_2}}     \mathbf{M}_{ij} g_{f2j} \bigg] -14.828 \Bigg) \times 10^3,\\
    \Delta H_{v,\textit{stp},i} &= \Bigg( \bigg[ \sum_{k = 1}^{N_{g_1}} \mathbf{N}_{ik} h_{v1k} + W                      \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij} h_{v2j} \bigg] + 6.829\Bigg) \times 10^3, \\
    \omega_i &= 0.4085 \ln  \bigg( \Big[  \sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} \omega_{1k} + W                  \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij} \omega_{2j} + 1.1507\Big]^{1/0.5050} \bigg), \label{eq:gcm-omega}\\
    V_{m,\textit{stp},i} &= \Bigg( \bigg[ \sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} v_{m1k} + W \sum_{j=1}^{N_{g_2}}          \mathbf{M}_{ij} v_{m2j} \bigg] + 0.01211 \Bigg)\times 10^{-3}, \\
    C_{p,\textit{stp},i} & =\bigg[\sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} C_{pA1_k} + W \sum_{j=1}^{N_{g_2}}                \mathbf{M}_{ij} C_{pA2_j} -19.7779\bigg]  \nonumber \\
        & +\bigg[\sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} C_{pB1_k} + W \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij} C_{pB2_j} + 22.5981\bigg] \theta \nonumber\\
        & +\bigg[\sum_{k=1}^{N_{g_1}} \mathbf{N}_{ik} C_{pC1_k} + W \sum_{j=1}^{N_{g_2}} \mathbf{M}_{ij} C_{pC2_j} - 10.7983\bigg] \theta^2 \\
    \theta &= \frac{T - 298.15}{700}
    \end{align*}

.. _eq-GCM-correlations:

Equations for individual compound correlations
----------------------------------------------

This section presents correlations for physical properties that leverage the individual compound properties defined in :ref:`eq-GCM-properties`.  These correlations make it possible to evaluate physical properties at non-standard temperatures and pressures, given that group contribution properties are only defined at standard conditions. The :ref:`tab-dimensionless-qtys` are used throughout this section for each compound *i*, provided :math:`T` in :math:`^{\circ}` K unless noted otherwise.

.. _tab-correlation-qtys:

.. table:: Derived quantities and temperature corrections
   :widths: auto
   :align: center

   =============================  =====================  ===============================================================
   Property                       Units                  Description
   =============================  =====================  ===============================================================
   :math:`\nu_i`                  m\ :sup:`2`\ /s        Kinematic viscosity\ :footcite:p:`viswanath_viscosity_2007`.
   :math:`L_{v,\textit{stp},i}`   J/kg                   Latent heat of vaporization at 298 K\ :footcite:p:`govindaraju_group_2016`.
   :math:`L_{v,i}`                J/kg                   Temperature-adjusted latent heat of vaporization at 298 K\ :footcite:p:`govindaraju_group_2016`.
   :math:`V_{m,i}`                m\ :sup:`3`\ /mol      Temperature-adjusted liquid molar volume\ :footcite:p:`rackett_equation_1970,yamada_saturated_1973,govindaraju_group_2016`.
   :math:`C_{\ell,i}`             J/kg/K                 Liquid specific heat capacity\ :footcite:p:`govindaraju_group_2016`. 
   :math:`p_{sat,i}`              Pa                     Saturated vapor pressure\ :footcite:p:`lee_generalized_1975,ambrose_vapour_1989`.
   =============================  =====================  ===============================================================


.. _tab-dimensionless-qtys:

.. table:: Reduced temperature quantities
   :widths: auto
   :align: center

   =============================  =========================================  ======================================================
   Symbol                         Definition                                 Description
   =============================  =========================================  ======================================================
   :math:`T_{r,i}`                :math:`\frac{T}{T_{c,i}}`                  Reduced temperature.
   :math:`T_{r,b,i}`              :math:`\frac{T}{T_{b,i}}`                  Reduced temperature relative to normal boiling point.
   :math:`T_{r,\textit{stp},i}`   :math:`\frac{298 \text{ (K)}}{T_{c,i}}`    Reduced temperature relative to standard temperature.
   =============================  =========================================  ======================================================

Kinematic viscosity
^^^^^^^^^^^^^^^^^^^
The kinematic viscosity of the *i-th* compound of the fuel, 

.. math::
   
   \nu_i = \frac{\mu_i}{\rho_i}, 

is calculated from Dutt's equation (Eq. 4.23 in Viscosity of 
Liquids\ :footcite:p:`viswanath_viscosity_2007`) provided :math:`T` in :math:`^{\circ}` C:

.. math::

   \begin{align*}
   \nu_i = 10^{-6} \times \exp \bigg\{-3.0171 + \frac{442.78 + 1.6452 \,T_{b,i}}{T + 239 - 0.19 \,T_{b,i}} \bigg\}.
   \end{align*}

Latent heat of vaporization
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The latent heat of vaporization for each compound at standard pressure and 
temperature is calculated from the enthalpy of vaporization as:

.. math::
   L_{v,\textit{stp},i} = \frac{\Delta H_{v,\textit{stp},i}}{M_{w,i}}.

The heat of vaporization for each compound is then adjusted for variations in 
temperature\ :footcite:p:`govindaraju_group_2016`:

.. math::
   L_{v,i} = L_{v,\textit{stp},i} \bigg(\frac{1 - T_{r,i}}{1-T_{r,b,i}} \bigg)^{0.38}.



Liquid molar volume
^^^^^^^^^^^^^^^^^^^

The liquid molar volume is calculated at a specific temperature :math:`T` using 
the generalized Rackett equation\ :footcite:p:`rackett_equation_1970,yamada_saturated_1973` 
with an updated :math:`\phi_i` parameter\ :footcite:p:`govindaraju_group_2016`:

.. math::

   V_{m,i} = V_{m,\textit{stp},i} Z^{\phi_i}_{c,i}, 

where

.. math::
   \begin{align*}
   Z_{c,i} &= 0.29056 - 0.08775 \omega_i,  \\
   \phi_i &= 
   \begin{cases}
       (1 - T_{r,i})^{2/7} - (1 - T_{r,\textit{stp},i})^{2/7}, & \text{ if } T \leq T_{c,i} \\
       - (1 - T_{r,\textit{stp},i})^{2/7}, & \text{ if } T > T_{c,i}
   \end{cases}. \label{eq:phi}
   \end{align*}


Liquid specific heat capacity
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The liquid specific heat capacity for each compound at standard pressure temperature is calculated from the specific heat capacity as:

.. math::
   C_{\ell,i} = \dfrac{C_{p,\textit{stp},i}}{M_{w,i}} 



Saturated vapor pressure
^^^^^^^^^^^^^^^^^^^^^^^^

The saturated vapor pressure for each compound is calculated as a function of 
temperature using either the Lee–Kesler method\ :footcite:p:`lee_generalized_1975` 
or the Ambrose-Walton method\ :footcite:p:`ambrose_vapour_1989`.  Both methods solve

.. math::
   \ln p_{r,\text{sat},i} = f_i^{(0)} + \omega_i f_i^{(1)} + \omega_i^2 f_i^{(2)}

for the reduced saturated vapor pressure for each compound, 
:math:`p_{r,\text{sat},i} = p_{\text{sat},i}/p_{c,i}`.  
The default method in `FuelLib` is the Lee-Kesler method, as it is 
more stable at higher temperatures. 
The Lee-Kesler\ :footcite:p:`lee_generalized_1975` method defines

.. math::

   \begin{align*}
   f_i^{(0)} &= 5.92714 - \frac{6.09648}{T_{r,i}} - 1.28862 \ln T_{r,i} + 0.169347 \, T_{r,i}^6, \\
   f_i^{(1)} &= 15.2518 - \frac{15.6875}{T_{r,i}} - 13.4721 \ln T_{r,i} + 0.43577 \, T_{r,i}^6, \\
   f_i^{(2)} &= 0,
   \end{align*}

The Ambrose-Walton\ :footcite:p:`ambrose_vapour_1989` correlation sets:

.. math::
   \begin{align*}
   f_i^{(0)} &= \frac{- 5.97616\tau_i + 1.29874\tau_i^{1.5} - 0.60394\tau_i^{2.5} - 1.06841\tau_i^{5}}{T_{r,i}}, \\
   f_i^{(1)} &= \frac{- 5.03365\tau_i + 1.11505\tau_i^{1.5} - 5.41217\tau_i^{2.5} - 7.46628\tau_i^{5},}{T_{r,i}}, \\
   f_i^{(2)} &= \frac{- 0.64771\tau_i + 2.41539\tau_i^{1.5} - 4.26979\tau_i^{2.5} - 3.25259\tau_i^{5}}{T_{r,i}},
   \end{align*}

with :math:`\tau_i = 1 - T_{r,i}`.


.. _eq-mixture-properties:

Equations for mixture properties from GCM
-----------------------------------------

This section contains correlations for estimating physical properties of the 
mixture from the individual compound and physical properties defined in 
:ref:`eq-GCM-properties` and :ref:`eq-GCM-correlations`.  These correlations make 
it possible to evaluate physical properties at non-standard temperatures and 
pressures, given that group contribution properties are only defined at standard 
conditions. The :ref:`tab-mixture-properties` available in `FuelLib` are listed in 
table below.  Mass and mole fractions defined in Table \ref{tab:mass-mole-fracs} 
are used throughout this section.

.. _tab-mixture-properties:

.. table:: Mixture properties
   :widths: auto
   :align: center
   
   =============  ===============  =====================
   Symbol         Units            Description
   =============  ===============  =====================
   :math:`\rho`   kg/m\ :sup:`3`   Density
   :math:`\nu`    m\ :sup:`2`/s    Kinematic viscosity
   :math:`p_v`    Pa               Vapor pressure
   =============  ===============  =====================

.. table:: Mass and mole fractions
   :widths: auto
   :align: center

   =============  ========================================  ==================================================================================
   Symbol         Definition                                Description
   =============  ========================================  ==================================================================================
   :math:`Y_i`    :math:`\frac{m_i}{\sum_{k=1}^{N_c} m_k}`   Mass fraction of compound *i*. :math:`m_i` is the mass of compound *i*.
   :math:`X_i`    :math:`\frac{n_i}{\sum_{k=1}^{N_c} n_k}`   Mole fraction of compound *i*. :math:`n_i` is the number of moles compound *i*.
   =============  ========================================  ==================================================================================

Mixture density
^^^^^^^^^^^^^^^
The mixture's density is calculated as:

.. math::
   
   \rho = \sum_{i=1}^{N_c}Y_i\frac{M_{w,i}}{V_{m,i}}.


Mixture kinematic viscosity
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The kinematic viscosity of the mixture is computed using the Kendall-Monroe\ :footcite:p:`kendall_viscosity_1917` 
mixing rule, with an option to use the Arrhenius\ :footcite:p:`arrhenius_uber_1887` 
mixing rule. The viscosity of each component.  Hernandez et al.\ :footcite:p:`hernandez_evaluation_2021` 
found, after evaluating thirty mixing rules, that both Kendall-Monroe and Arrhenius 
were among the most effective without relying on additional data or parameter fitting. 
The Kendall-Monroe rule is: 

.. math::

   \nu_{KM}^{1/3} = \sum_{i=1}^{N_c} X_i \, \nu_i^{1/3}. 

The Arrhenius rule is:

.. math::

   \ln \nu_{Arr} = \sum_{i=1}^{N_c} X_i\ln\nu_i .



Mixture vapor pressure
^^^^^^^^^^^^^^^^^^^^^^

The vapor pressure of the mixture is calculated according to Raoult's law:

.. math::
   \begin{align*}
   p_{v} = \sum_{i = 1}^{N_c} X_i \, p_{\textit{sat},i}.
   \end{align*}

Mixture property validation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. image:: /figures/mixtureProps-decane.png
   :width: 600pt
   :align: center

.. image:: /figures/mixtureProps-dodecane.png
   :width: 600pt
   :align: center

.. image:: /figures/mixtureProps-heptane.png
   :width: 600pt
   :align: center
   
Mixture properties of decane, dodecane, and heptane.  Data from NIST Chemistry WebBook.


.. image:: /figures/mixtureProps-posf10325.png
   :width: 600pt
   :align: center

Density (left) and vapor pressure (right) for POSF-10325 against data from the Air Force Research Laboratory\ :footcite:p:`edwards_jet_2020`.

References
----------

.. footbibliography::