# PQC Algorithms

Notes: XMSS-SHAKE_20_128_w256 has tiny 676byte signatures, but only allows a
million signatures. That should be sufficient for a root `CA. XMSS-SHAKE_20_128`
(note the missing `_w256`), also only has a million signatures, which are a bit
larger 900byte, but signing and verifying are 8 times quicker. The latter, the
faster one, verifies in 180µs (in Go), which take 62µs for Ed25519 verification.
In comparison the smallest SHAKE variant of SPHINCS+ (with 8KB signatures) takes
600,000µs for signing and 665µs for verifying. So in this specific case, the
penalty of getting rid of the state is 10x larger signatures, ~5x slower
verification and glacially slow signing (signing speed can't directly be
compared with XMSS as the latter can do caching during keygen, but it's much,
much faster).

| Algorithm  | Signing Time | Verifying Time | Certificate Size | Signature Size | Public Key Size | State (Cache) Size |
|------------|--------------|----------------|------------------|----------------|-----------------|--------------------|
| Dilithium3 |	Fast	    |    Fast	     |                  |  2.7KB	 |     1.4KB	   |                    |
| Falcon     |	Medium/Slow |	 Fast	     |                  |  1.2KB	 |     1.7KB	   |                    |
| XMSS[xt]   |  Fast	    |    Fast/Medium |                  |  (tradeoff)	 |   (tradeoff)	   |      (tradeoff)    |
| SPHINCS+   |	Slow	    |    Slow	     |                  |   8-32KB	 |     32B	   |                    |

The round 3 candidates [have been announced](https://csrc.nist.gov/News/2020/pqc-third-round-candidate-announcement),
so we should analyse:

* The Finalists:

  * Public-Key Encryption/KEMs

    * [Classic McEliece](https://classic.mceliece.org/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/Classic-McEliece-round2-official-comment.pdf).
    * [CRYSTALS-KYBER](https://pq-crystals.org/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/CRYSTALS-KYBER-round2-official-comment.pdf).
    * [NTRU](https://ntru.org/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/NTRU-round2-official-comment.pdf).
    * [SABER](https://www.esat.kuleuven.be/cosic/pqcrypto/saber/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/SABER-round2-official-comment.pdf).

  * Digital Signatures

    * [CRYSTALS-DILITHIUM](https://pq-crystals.org/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/CRYSTALS-DILITHIUM-round2-official-comment.pdf).
    * [FALCON](https://falcon-sign.info/). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/FALCON-round2-official-comment.pdf).
    * [Rainbow](https://sites.google.com/site/jintaiding/nist-papers). Official comments [here](https://csrc.nist.gov/CSRC/media/Projects/post-quantum-cryptography/documents/round-2/official-comments/Rainbow-round2-official-comment.pdf).

## Libraries

* [liboqs](https://openquantumsafe.org/). The library can be found [here in C](https://github.com/open-quantum-safe/liboqs), and [here in Golang](https://github.com/open-quantum-safe/liboqs-go).
* [pqclean](https://github.com/PQClean/PQClean).

