---
layout: page
title: About LIMA
permalink: /about/
---

We introduce LIMA (LattIce MAthematics), a set of lattice-based public-key encryption and key encapsulation mechanisms, offering chosen plaintext security and chosen ciphertext security options. LIMA mixes conservative, standard and boring design choices with some efficiency improvements and flexibility. These factors are exhibited in its genesis: it is based on the ring variant [<a href="#EC:LyuPeiReg10">EC:LyuPeiReg10</a>] of the LWE problem [<a href="#STOC:Regev05">STOC:Regev05</a>] and on the encryption construction in [<a href="#RSA:LinPei11">RSA:LinPei11</a>]. We use the Fujisaki-Okamoto transform [<a href="#PKC:FujOka99">PKC:FujOka99</a>] to obtain an IND-CCA secure public-key encryption scheme. Our IND-CCA key encapsulation mechanism (KEM) is obtained via a transform of Dent [<a href="#IMA:Dent03">IMA:Dent03</a>]. This provides improved communication efficiency over using our IND-CCA public-key encryption scheme directly as a KEM; we also give a tight security proof for our IND-CCA KEM.

Thus our basic building blocks use highly respected and well studied cryptographic components. Our preference for “boring and simple” is illustrated by the fact that while our construction is efficient, other constructions such as [<a href="#EPRINT:BDKLLS17">EPRINT:BDKLLS17</a>] achieve higher encryption and decryption speeds. However, run times for lattice-based schemes are already generally faster than current public-key schemes and thus we view optimizing run times as being less important compared to simplicity.

## Mathematical Structure

For efficiency we use a ring variant. Encryption based on Ring-LWE has been extensively studied in the literature [<a href="#ACISP:BaiGal14">ACISP:BaiGal14</a>; <a href="#EC:LyuPeiReg10">EC:LyuPeiReg10</a>; <a href="#EC:LyuPeiReg13">EC:LyuPeiReg13</a>; <a href="#CMVRCPV15">CMVRCPV15</a>]. The core component is essentially the Lyubashevsky *et al.* scheme [<a href="#EC:LyuPeiReg10">EC:LyuPeiReg10</a>], or (equivalently) the BGV [<a href="#BraGenVai14">BraGenVai14</a>] homomorphic encryption scheme, with the message being in the upper bits as opposed to the lower bits. This is sometimes referred to as the FV scheme [<a href="#EPRINT:FanVer12">EPRINT:FanVer12</a>].[^1] The scheme also bears some resemblance to the scheme in [<a href="#EPRINT:Peikert14">EPRINT:Peikert14</a>], although we adopt a completely different approach to ciphertext compression and producing IND-CCA variants. A lot of prior research has been conducted not only into the basic scheme, but also into implementation aspects [<a href="#CHES:LSRGKV15">CHES:LSRGKV15</a>; <a href="#CHES:RVMCV14">CHES:RVMCV14</a>], including protecting against side-channel attacks [<a href="#CHES:RRVV15">CHES:RRVV15</a>].

We opted for the ring variant of LWE to reduce the ciphertext size and to increase performance compared to LWE. In between Ring-LWE and LWE sits the Module-LWE [<a href="#Langlois:2015:WCA">Langlois:2015:WCA</a>] problem. As recently shown in [<a href="#EPRINT:AlbDeo17">EPRINT:AlbDeo17</a>], this problem is polynomial-time equivalent to Ring-LWE with a large modulus. Such large modulus Ring-LWE instances are distinguished from our Ring-LWE instances by the module-rank required to solve them. That is, while the Ring-LWE problem considered here requires one to find unusually short vectors in a module of rank three, this dimension is bigger in constructions such as [<a href="#EPRINT:BDKLLS17">EPRINT:BDKLLS17</a>]. At present, it is unclear if any loss of security is implied by using smaller module rank for ranks $> 1$.

We offer two forms of ring to use within the LIMA system:

-   Power-of-two Cyclotomics: These are relatively well studied and admit very efficient implementations.

-   Safe-Prime Cyclotomics: These are almost as good as power-of-two cyclotomics in terms of the ring geometry, but are slightly less efficient.

The reasons for offering the second option for the ring are twofold. Firstly, using safe-prime cyclotomics allows us to create a more flexible parameter space in terms of the ring dimension. Secondly, recall that a prime $p$ is called “safe” if $p=2 \cdot p'+1$ for another prime $p'$. This implies that there are only two non-trivial subfields of the cyclotomic field $\mathbf{Q}[X]/\Phi_p(X)$. By contrast, the presence of subfield towers in the power-of-two case raises the question of whether this could leave open routes for attack such as those in [<a href="#CheJeoLee16">CheJeoLee16</a>; <a href="#C:AlbBaiDuc16">C:AlbBaiDuc16</a>]. While it was shown soon after that towers of subfields are not required for these attacks to succeed [<a href="#EC:KirFou17">EC:KirFou17</a>], it appears prudent to hedge against possible future attacks regardless. For example, [<a href="#EC:BBVLV17">EC:BBVLV17</a>] shows that, for some fields, the presence of subfields can lead to a much easier lattice problem. We stress, however, that none of these subfield attacks are currently applicable to Ring-LWE. In summary, for added flexibility and to allow a more conservative choice of field, we give the *option* of using safe-prime cyclotomics.

The use of safe-primes appears to mitigate the possibility of attacks via subfields, but it also avoids the complex ring geometry associated with picking fields with large Galois group, such as the choice made in [<a href="#EPRINT:BCLV16">EPRINT:BCLV16</a>]. Thus, we believe that using safe-prime fields offers a nice compromise between efficiency and protection against potential future attacks. In addition, using safe-prime cyclotomics allows us, with a little extra work, to be able to still use FFTs to evaluate the main polynomial products involved in the operation of our schemes. Thus, we think safe prime cyclotomic fields offer a good compromise between the benefits of the use of two power cyclotomics and fields defined by polynomials with large Galois groups.

We stress that we consider our schemes instantiated with the power-of-two rings to be secure, and they are more efficient in this case. We only offer the safe-prime variant if subfield attacks are a concern, and to offer additional flexibility in terms of parameter choices.

## Cryptographic Structure

We offer IND-CPA and IND-CCA variants of a public-key encryption scheme. The IND-CCA variant is based on the Fujisaki-Okamoto transform [<a href="#PKC:FujOka99">PKC:FujOka99</a>]. Our choice of this methodology for achieving IND-CCA security is that it permits a tight security proof. We also offer IND-CPA and IND-CCA variants of a KEM construction, with the IND-CCA variant being built using a construction of Dent [<a href="#IMA:Dent03 Table 5">IMA:Dent03 Table 5</a>] applied to our IND-CPA public-key encryption scheme. Dent’s transformation did not have a tight security proof, so in [<a href="#ES:AOPPS17">ES:AOPPS17</a>] we also provide a new proof that is tight in our specific context. We note that a related but more generic tight reduction was recently given in [<a href="#EPRINT:HofHovKil17">EPRINT:HofHovKil17</a>].

Ignoring parameter sizes for the moment, this document therefore defines eight possible configurations for LIMA, as listed in the table below.

| Name                 | Ring Variant   | Functionality       | Security   |
| -------------------- | -------------- | ------------------- | ---------- |
| **LIMA-2p-Enc-CPA**  | Two-Power      | Encryption          | IND-CPA    |
| **LIMA-sp-Enc-CPA**  | Safe-Prime     | Encryption          | IND-CPA    |
| **LIMA-2p-KEM-CPA**  | Two-Power      | Key Encapsulation   | IND-CPA    |
| **LIMA-sp-KEM-CPA**  | Safe-Prime     | Key Encapsulation   | IND-CPA    |
| **LIMA-2p-Enc-CCA**  | Two-Power      | Encryption          | IND-CCA    |
| **LIMA-sp-Enc-CCA**  | Safe-Prime     | Encryption          | IND-CCA    |
| **LIMA-2p-KEM-CCA**  | Two-Power      | Key Encapsulation   | IND-CCA    |
| **LIMA-sp-KEM-CCA**  | Safe-Prime     | Key Encapsulation   | IND-CCA    |

  : The different LIMA schemes.


The first two schemes in Table \[table-lima-schemes\] should not be used (without care) in any application. The third and fourth schemes should also be used with care, but are included here as they are mentioned in the NIST call as being potentially desirable in some key exchange applications.

All the LIMA schemes are derived from a base encryption algorithm `EncCPASub` which can result in decryption failures. In particular `EncCPASub` could make a choice of randomness that produces in a ciphertext which results in a decryption failure (or even decrypts to the wrong message). This is a standard problem in lattice based schemes and can cause problems in operation and in security proofs. To avoid this issue, we then modify the base encryption algorithm `EncCPASub` so that it rejects any choice of randomness which *could* result in an eventual decryption failure. Thus, we apply rejection sampling at the encryption stage so as to remove the problem of decryption failures. By choosing our parameters carefully, we can make the probability that repeated sampling is needed during encryption relatively small, thus avoiding in almost all cases the need to repeat the encryption procedure. This results in a non-constant time encryption algorithm, though it will be constant time with high probability and will not leak any information about the message. We discuss this further below.

We define six parameter sets to be used in conjunction with the eight different LIMA schemes. Two of these parameter sets are in the “power-of-two” setting and four in the “safe-prime” setting. This yields a total of 24 different schemes, with this flexibility enabling scaling to larger messages spaces and/or increased security levels. The six parameter sets are shown in the table below.

| Scheme      | $N$    | $q$        |
| --------    | ------ | ---------- |
| **LIMA-2p** | 1024   | 133121     |
| **LIMA-2p** | 2048   | 184321     |
| **LIMA-sp** | 1018   | 12521473   |
| **LIMA-sp** | 1306   | 48181249   |
| **LIMA-sp** | 1822   | 44802049   |
| **LIMA-sp** | 2062   | 16900097   |

  : Parameter sets for use with LIMA.


Note that for LIMA-sp parameter sets, the larger value of $q$ is to enable the FFT to be efficiently computed via Bluestein’s algorithm. This places some requirements on the value of $q$. Using the FFT also means that ring multiplication can be more easy implemented on highly parallel processors such as GPUs. Another option in the LIMA-sp case would have been to abandon the usage of the FFT methods and use smaller values of $q$. This, however, would have come at the expense of an estimated slowdown by a factor of about two.[^2]

## Random Seeds

All random seeds/coins for our algorithms are passed through a NIST approved XOF, from NIST SP 800 185 [<a href="#NIST:SP800185">NIST:SP800185</a>]. This means *we do not* pass the seeds/coins through a DRBG first, as recommended by NIST (in the FAQ related to this call).

The XOF output is used to generate random finite field elements, symmetric keys, and (importantly for us) samples from a distribution somewhat close to a Gaussian distribution. For this latter task we adopt a method suggested in [<a href="#USENIX:ADPS16">USENIX:ADPS16</a>], which is both efficient (in software and hardware) and constant-time.

## Ciphertext Compression

All ciphertexts are compressed in the sense of ignoring *coefficients* of the “message carrying component” which contains no information about the message. Further compression can be obtained by Huffman encoding the ring elements.

In the case of our IND-CPA KEM, a further form of compression is possible by utilizing the reconciliation approach of [<a href="#EPRINT:Peikert14">EPRINT:Peikert14</a>]. We did not use this for two reasons. Firstly, it is only applicable to the IND-CPA KEM.[^3] Secondly, there is some concern in the community over patenting of reconciliation mechanisms, and we wanted to avoid uncertainties arising from unresolved issues relating to the licensing of patents.

## Intellectual Property

To our knowledge, the algorithms contained in this proposal are not covered by any intellectual property constraints.

The implementations provided in this proposal were written by Valery Osheter, Guy Peer and Nigel P. Smart.

## References

<div id="refs" class="references">
  <a name="EPRINT:AlbDeo17"><div id="ref-EPRINT:AlbDeo17">
    <p>Albrecht, Martin R., and Amit Deo. 2017. “Large Modulus Ring-LWE &gt;= Module-LWE.” Cryptology ePrint Archive, Report 2017/612.
    </p>
  </div></a>
  <a name="C:AlbBaiDuc16"><div id="ref-C:AlbBaiDuc16">
    <p>Albrecht, Martin R., Shi Bai, and Léo Ducas. 2016. “A Subfield Lattice Attack on Overstretched NTRU Assumptions - Cryptanalysis of Some FHE and Graded Encoding Schemes.” In
      <em>CRYPTO 2016, Part I
      </em>, edited by Matthew Robshaw and Jonathan Katz, 9814:153–78. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-662-53018-4_6">10.1007/978-3-662-53018-4_6
      </a>.
    </p>
  </div></a>
  <a name="ES:AOPPS17"><div id="ref-ES:AOPPS17">
    <p>Albrecht, Martin R., Emmanuela Orsini, Kenneth G. Paterson, Guy Peer, and Nigel P. Smart. 2017. “Tightly Secure Ring-Lwe Based Key Encapsulation with Short Ciphertexts.” In
      <em>Computer Security - ESORICS 2017 - 22nd European Symposium on Research in Computer Security, Oslo, Norway, September 11-15, 2017, Proceedings, Part I
      </em>, edited by Simon N. Foley, Dieter Gollmann, and Einar Snekkenes, 10492:29–46. Lecture Notes in Computer Science. Springer. doi:
      <a href="https://doi.org/10.1007/978-3-319-66402-6_4">10.1007/978-3-319-66402-6_4
      </a>.
    </p>
  </div></a>
  <a name="USENIX:ADPS16"><div id="ref-USENIX:ADPS16">
    <p>Alkim, Erdem, Léo Ducas, Thomas Pöppelmann, and Peter Schwabe. 2016. “Post-Quantum Key Exchange - A New Hope.” In
      <em>25th USENIX Security Symposium, USENIX Security 16, Austin, Tx, Usa, August 10-12, 2016.
      </em>, edited by Thorsten Holz and Stefan Savage, 327–43. USENIX Association.
      <a href="https://www.usenix.org/conference/usenixsecurity16/
               technical-sessions/presentation/alkim" class="uri">https://www.usenix.org/conference/usenixsecurity16/
        technical-sessions/presentation/alkim
      </a>.
    </p>
  </div></a>
  <a name="ACISP:BaiGal14"><div id="ref-ACISP:BaiGal14">
    <p>Bai, Shi, and Steven D. Galbraith. 2014. “Lattice Decoding Attacks on Binary LWE.” In
      <em>ACISP 14
      </em>, edited by Willy Susilo and Yi Mu, 8544:322–37. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-319-08344-5_21">10.1007/978-3-319-08344-5_21
      </a>.
    </p>
  </div></a>
  <a name="EC:BBVLV17"><div id="ref-EC:BBVLV17">
    <p>Bauch, Jens, Daniel J. Bernstein, Henry de Valence, Tanja Lange, and Christine van Vredendaal. 2017. “Short Generators Without Quantum Computers: The Case of Multiquadratics.” In
      <em>EUROCRYPT 2017, Part I
      </em>, edited by Jean-Sébastien Coron and Jesper Buus Nielsen, 10210:27–59. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="EPRINT:BCLV16"><div id="ref-EPRINT:BCLV16">
    <p>Bernstein, Daniel J., Chitchanok Chuengsatiansup, Tanja Lange, and Christine van Vredendaal. 2016. “NTRU Prime.” Cryptology ePrint Archive, Report 2016/461.
    </p>
  </div></a>
  <a name="EPRINT:BDKLLS17"><div id="ref-EPRINT:BDKLLS17">
    <p>Bos, Joppe, Léo Ducas, Eike Kiltz, Tancrède Lepoint, Vadim Lyubashevsky, John M. Schanck, Peter Schwabe, and Damien Stehlé. 2017. “CRYSTALS – Kyber: A CCA-Secure Module-Lattice-Based KEM.” Cryptology ePrint Archive, Report 2017/634.
    </p>
  </div></a>
  <a name="BraGenVai14"><div id="ref-BraGenVai14">
    <p>Brakerski, Zvika, Craig Gentry, and Vinod Vaikuntanathan. 2014. “(Leveled) Fully Homomorphic Encryption Without Bootstrapping.”
      <em>TOCT
      </em> 6 (3): 13:1–13:36. doi:
      <a href="https://doi.org/10.1145/2633600">10.1145/2633600
      </a>.
    </p>
  </div></a>
  <a name="CMVRCPV15"><div id="ref-CMVRCPV15">
    <p>Chen, Donald Donglong, Nele Mentens, Frederik Vercauteren, Sujoy Sinha Roy, Ray C. C. Cheung, Derek Pao, and Ingrid Verbauwhede. 2015. “High-Speed Polynomial Multiplication Architecture for Ring-LWE and SHE Cryptosystems.”
      <em>IEEE Trans. on Circuits and Systems
      </em> 62-I (1): 157–66. doi:
      <a href="https://doi.org/10.1109/TCSI.2014.2350431">10.1109/TCSI.2014.2350431
      </a>.
    </p>
  </div></a>
  <a name="CheJeoLee16"><div id="ref-CheJeoLee16">
    <p>Cheon, Jung Hee, Jinhyuck Jeong, and Changmin Lee. 2016. “An Algorithm for Ntru Problems and Cryptanalysis of the Ggh Multilinear Map Without a Low-Level Encoding of Zero.”
      <em>LMS Journal of Computation and Mathematics
      </em> 19 (A). London Mathematical Society: 255–66. doi:
      <a href="https://doi.org/10.1112/S1461157016000371">10.1112/S1461157016000371
      </a>.
    </p>
  </div></a>
  <a name="IMA:Dent03"><div id="ref-IMA:Dent03">
    <p>Dent, Alexander W. 2003. “A Designer’s Guide to KEMs.” In
      <em>9th Ima International Conference on Cryptography and Coding
      </em>, edited by Kenneth G. Paterson, 2898:133–51. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="EPRINT:FanVer12"><div id="ref-EPRINT:FanVer12">
    <p>Fan, Junfeng, and Frederik Vercauteren. 2012. “Somewhat Practical Fully Homomorphic Encryption.” Cryptology ePrint Archive, Report 2012/144.
    </p>
  </div></a>
  <a name="PKC:FujOka99"><div id="ref-PKC:FujOka99">
    <p>Fujisaki, Eiichiro, and Tatsuaki Okamoto. 1999. “How to Enhance the Security of Public-Key Encryption at Minimum Cost.” In
      <em>PKC’99
      </em>, edited by Hideki Imai and Yuliang Zheng, 1560:53–68. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="EPRINT:HofHovKil17"><div id="ref-EPRINT:HofHovKil17">
    <p>Hofheinz, Dennis, Kathrin Hövelmanns, and Eike Kiltz. 2017. “A Modular Analysis of the Fujisaki-Okamoto Transformation.” Cryptology ePrint Archive, Report 2017/604.
    </p>
  </div></a>
  <a name="EC:KirFou17"><div id="ref-EC:KirFou17">
    <p>Kirchner, Paul, and Pierre-Alain Fouque. 2017. “Revisiting Lattice Attacks on Overstretched NTRU Parameters.” In
      <em>EUROCRYPT 2017, Part I
      </em>, edited by Jean-Sébastien Coron and Jesper Buus Nielsen, 10210:3–26. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="Langlois:2015:WCA"><div id="ref-Langlois:2015:WCA">
    <p>Langlois, Adeline, and Damien Stehlé. 2015. “Worst-Case to Average-Case Reductions for Module Lattices.”
      <em>Designs, Codes &amp; Cryptography
      </em> 75 (3): 565–99. doi:
      <a href="https://doi.org/http://dx.doi.org/10.1007/s10623-014-9938-4">http://dx.doi.org/10.1007/s10623-014-9938-4
      </a>.
    </p>
  </div></a>
  <a name="RSA:LinPei11"><div id="ref-RSA:LinPei11">
    <p>Lindner, Richard, and Chris Peikert. 2011. “Better Key Sizes (and Attacks) for LWE-Based Encryption.” In
      <em>CT-Rsa 2011
      </em>, edited by Aggelos Kiayias, 6558:319–39. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="CHES:LSRGKV15"><div id="ref-CHES:LSRGKV15">
    <p>Liu, Zhe, Hwajeong Seo, Sujoy Sinha Roy, Johann Großschädl, Howon Kim, and Ingrid Verbauwhede. 2015. “Efficient Ring-LWE Encryption on 8-Bit AVR Processors.” In
      <em>CHES 2015
      </em>, edited by Tim Güneysu and Helena Handschuh, 9293:663–82. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-662-48324-4_33">10.1007/978-3-662-48324-4_33
      </a>.
    </p>
  </div></a>
  <a name="EC:LyuPeiReg10"><div id="ref-EC:LyuPeiReg10">
    <p>Lyubashevsky, Vadim, Chris Peikert, and Oded Regev. 2010. “On Ideal Lattices and Learning with Errors over Rings.” In
      <em>EUROCRYPT 2010
      </em>, edited by Henri Gilbert, 6110:1–23. LNCS. Springer, Heidelberg.
    </p>
  </div></a>
  <a name="EC:LyuPeiReg13"><div id="ref-EC:LyuPeiReg13">
    <p>———. 2013. “A Toolkit for Ring-LWE Cryptography.” In
      <em>EUROCRYPT 2013
      </em>, edited by Thomas Johansson and Phong Q. Nguyen, 7881:35–54. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-642-38348-9_3">10.1007/978-3-642-38348-9_3
      </a>.
    </p>
  </div></a>
  <a name="NIST:SP800185"><div id="ref-NIST:SP800185">
    <p>NIST National Institute for Standards and Technology. 2016. “SHA-3 Derived Functions: cSHAKE, KMAC, TupleHash and ParallelHash.”
    </p>
  </div></a>
  <a name="EPRINT:Peikert14"><div id="ref-EPRINT:Peikert14">
    <p>Peikert, Chris. 2014. “Lattice Cryptography for the Internet.” Cryptology ePrint Archive, Report 2014/070.
    </p>
  </div></a>
  <a name="STOC:Regev05"><div id="ref-STOC:Regev05">
    <p>Regev, Oded. 2005. “On Lattices, Learning with Errors, Random Linear Codes, and Cryptography.” In
      <em>37th Acm Stoc
      </em>, edited by Harold N. Gabow and Ronald Fagin, 84–93. ACM Press.
    </p>
  </div></a>
  <a name="CHES:RRVV15"><div id="ref-CHES:RRVV15">
    <p>Reparaz, Oscar, Sujoy Sinha Roy, Frederik Vercauteren, and Ingrid Verbauwhede. 2015. “A Masked Ring-LWE Implementation.” In
      <em>CHES 2015
      </em>, edited by Tim Güneysu and Helena Handschuh, 9293:683–702. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-662-48324-4_34">10.1007/978-3-662-48324-4_34
      </a>.
    </p>
  </div></a>
  <a name="CHES:RVMCV14"><div id="ref-CHES:RVMCV14">
    <p>Roy, Sujoy Sinha, Frederik Vercauteren, Nele Mentens, Donald Donglong Chen, and Ingrid Verbauwhede. 2014. “Compact Ring-LWE Cryptoprocessor.” In
      <em>CHES 2014
      </em>, edited by Lejla Batina and Matthew Robshaw, 8731:371–91. LNCS. Springer, Heidelberg. doi:
      <a href="https://doi.org/10.1007/978-3-662-44709-3_21">10.1007/978-3-662-44709-3_21
      </a>.
    </p>
  </div></a>
</div>

## Footnotes

[^1]: We do not use any homomorphic properties in this document, we just mention this to show the theoretical pedigree and prior analysis of our approach.

[^2]: In this case we would recommend using Karatsuba multiplication to multiply the ring elements, as opposed to coordinate wise multiplication in the FFT domain.

[^3]: In [<a href="#EPRINT:Peikert14">EPRINT:Peikert14</a>] a method of obtaining an IND-CCA secure KEM is given, but it is less efficient than ours.

