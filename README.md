This is my undergraduate thesis from the University of the Philippines Baguio, under a month and a half supervision
of my Adviser and Co-Adviser, Dr. Ian Jasper A. Agulo and Dr. Christian P. Crisostomo respectively. This was a continuation
of the study done by my friend and senior, Adoniram Balagtas.

The study aims to address challenges in computational chemistry, where solving the many-body Schrödinger equation
for molecules like CO₂ (with 22 electrons) is computationally intensive. DFT simplifies this by using electron density
as the key variable, but approximations like pseudopotentials can introduce errors. Furthermore, approximating the
pseudopotentials include approximating wavefunctions which gets more computationally expensive the more complex 
the molecule is. By integrating Physics-Informed Neural Networks (PINNs) which incorporate physical constraints into the 
loss function, the thesis seeks to create transferable models for predicting electron density and ground-state energy, 
benchmarking on CO₂ as a starting point for triatomic systems. The scope is limited to 1D ground-state predictions, 
relying on DFT's inherent approximations.

The methodology is simple, generate electron densities with corresponding energies(ground state) using PySCF(a DFT tool) across
different bond lengths. Next, predict electron densities and energies using the PINNs model. To compare the benchmark PINN
model, it must achieve the followin:\
1. It must peak where the atomic nucleis of CO2 are located
2. It must yield the energy vs bond length curve that indicates the bond length with the lowest energy similar to that of the
   energy vs bond lenth curve produced by PySCF.

The architecture of the PINNs model is a fully connected dense neural network that inputs the grid where the molecule 
is constructed. There is gaussian intialization with learnable centers and an exponential decay function centered at each nuclei
to impose localization of wavefunctions. Tanh activation functions were chosen as wavefunctions inherently take positive and negative values.
The loss function is the most important part. Since in DFT we are trying to find the energy that minimizes the structure,
and in machine learning we are trying to minimize the loss function, I modified the loss function to represent the
total energy of the system such that the neural network minimize the energy in the loss function. Additionally, I imposed the preservation
of total number of electrons(22 for CO2) also in the loss function. The model outputs the electronic density that minimizes the 
loss function.

In conclusion, I was able to achieve (1) as the model was able to give a good qualitative approximation of the electronic densities
where it peaked at the center of the atomic nucleis of the 2 oxygen and 1 carbon atoms. The model also gave a minimum energy bond length
of 1.25 angstroms, where PySCF gave 1.20 angstroms which was close. However, the bond length vs energy curve of the model was off by 700
hartree compared to that of the curve produced by PySCF showing the model's limitation sicne it is only 1-dimensional.

**FURTHER IMPORVEMENTS**\
Other models such as COnvolutional Neural Networks (CNN) could be explored since CNNs are known for their efficiency in handling structured data,
such as spatial or grid-based representations of electron densities, they might offer a computationally cheaper alternative to the fully connected neural 
networks (FCNNs) used in PINNs. Furthermore, Orbital-Free Density Functional Theory (OF DFT) could be an alternative theoretical framework
for the imposition of the physical laws in the loss function as OFDFT eliminates the need for orbitals altogether and enabling linear scaling with system size.





