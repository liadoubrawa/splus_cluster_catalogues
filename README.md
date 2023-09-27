# S-PLUS cluster catalogues

This work introduces galaxy cluster catalogues from the fourth internal data release of the Southern Photometric Local Universe Survey (S-PLUS/iDR4). The catalogue is based on the available photometric data within the redshift range of 0.05 < z < 0.5. It includes five of the six main regions: HYDRA, MC, SPLUS-S, SPLUS-N and STRIPE82. SPLUS-D is intentionally excluded as it focuses primarily on the Milky Way disk. To identify galaxy cluster candidates, we utilize the density-based algorithm PZWav (Gonzalez 2014; Werner et al.2022), which identifies overdensities on fixed physical scales (within 0.5-1.4 Mpc). Comparison with tailored simulations demonstrates that our catalogue is 80% pure and 95% complete for clusters with richness greater than 4. Furthermore, we utilize photometric data to characterize the cluster sample by providing a galaxy membership catalogue, as well as estimates for richness and optical luminosity using our adaptive membership estimator (AME) algorithm.

![splus_meeting2023](https://github.com/liadoubrawa/splus_cluster_catalogues/assets/79430629/369549d6-6af5-4375-aa7b-f8bd780fd0b3)


# Output

Here, one can find:
- Galaxy catalogue ('{area_name}.fits') galaxies from the S-PLUS survey, with information: ID, sky positions, zml, r_auto, odds, PROB_GAL, PDF; selected as:
  - Selecting criteria (Value Added Catalogues)
    - PROB_GAL >= 0.5, SEX_FLAGS_r != 2, r_auto < 21 and 0 < zml < 0.5.

  - Due to tiles superpositions we remove close objects (within 10’’)  and  Δr_auto < 1.

- Cluster catalogue ('{area_name}_0.fits'), alongside ID, sky positions [RA,DEC], photometric redshift estimates, SNR (SNR>2), richness, radius, rank (PZWav output) we provide a characteristic radius, Rc, and subproduct examples that can be derived from the galaxies' membership probabilities, such as richness [R_ame], optical luminosity [opt_lum].

- Galaxy catalogue ('data_{area_name}.fits'), galaxies identified within Rc, with the S-PLUS information: ID, sky positions, zml, r_auto, odds, PROB_GAL, PDF; We add the membership probability for each galaxy to be a cluster member [Prob], the probability of belonging to a nearby substructure [Prob_sub] and the cluster identification number [clusterID].

- Possible substructure catalogue ('{area_name_0_substructures.fits}'). Sky positions of the substructure (estimated as the weighted mean cartesian value of the galaxies with Prob_sub), mean redshift estimate from the galaxies with Prob_sub, rich (as the sum of Prob_sub), center distance between optical identification and substructure coordinates, and cluster identification: sky positions, photo-z, richness (sum of Prob).

Due to the large area of the main regions, some of them are subdivided as MC → mc1 and mc2; SPLUS-S → splus_s1, splus_s2, splus_s3;

Results are given without cuts ('without_cuts') and with an absolute magnitude cut of M<-20.25 (in r-band) which ensures a volume-limited sample up to a redshift of z < 0.5 ('mag_limited').

# About the Adaptive Membership Estimator (AME)

In the following, we present the main steps:

(i) Remove obvious non-members by cutting out all galaxies outside a radius of 2.5 Mpc in the plane-of-the-sky, and 𝑧𝑐𝑙𝑢𝑠𝑡𝑒𝑟 ± 0.05.

(ii) Calculate the galaxy density profile. A characteristic radius (𝑅𝑐), will be defined as a break or “knee” in this profile.

(iii) Draw a photo-z PDF-based random redshift value for each galaxy within 𝑅𝑐.

(iv) Calculate the sample velocity dispersion with the drawn redshifts, after a 3𝜎 clipping process.

(v) Run HDBSCAN using the remaining galaxies. Input parameters are galaxy positions and RFAE (𝑅 < 𝑅𝑐), as the minimum cluster size parameter. The largest galaxy concentration is identified as part of the cluster (Prob), and nearby clumps are identified as possible substructures (Prob_sub).

(vi) Repeat N times the steps 3–5. The final probability is 𝑃𝑚𝑒𝑚 = 𝑁𝑚𝑒𝑚/𝑁.


# About FAE

In the following, we present the main steps:

(i) Select galaxies within a radius of 500 kpc, and redshift interval, 𝑧𝑐𝑙𝑢𝑠𝑡𝑒𝑟 ± 0.05.

(ii) Integrate the selected galaxies photo-z PDF within the redshift interval (Prob).

(iii) Calculate the richness (Ri) as the sum of Prob

(iv) Remove field contribution within the cluster area (Rfield)

(v) Final richness, RFAE = Ri - Rfield



