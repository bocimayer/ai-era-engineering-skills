# Neurodivergence Trait Profile

A single-page questionnaire (`index.html`, no build step, open it in a browser) that profiles measurable behavioural traits of ADHD and related neurodevelopmental differences: autism spectrum, sensory processing, specific learning disorders and developmental coordination disorder.

- **Adult form (18+)** is a self-report with 91 items across 24 dimensions. It uses the ASRS five-point frequency scale, scored 0–4.
- **Child form (9–10)** is a parent or caregiver report with 88 items across 23 dimensions. It uses the Vanderbilt/SNAP-IV four-point scale, scored 0–3. A teacher copy is recommended.
- **Languages:** English and Hungarian (Magyar), switchable at the top of the page. The page defaults to Hungarian when the browser language is Hungarian. The Hungarian items are translations of this questionnaire's items, not the official validated Hungarian adaptations of the source instruments. Reference citations stay in their original English.

> **Screening profile only, not a diagnosis.** Items are paraphrased adaptations written around the constructs of validated instruments, and several of those instruments are copyrighted. Scores are not normed against a population sample.

## Research basis: measurable dimensions and their instruments

| Domain | Dimension | Evidence / instruments |
|---|---|---|
| Attention & activity | Inattention (9 DSM-5 symptoms) | DSM-5-TR A1; WHO ASRS v1.1 (Kessler 2005); ASRS-5 (Ustun 2017); Vanderbilt (Wolraich 2003); SNAP-IV (Bussing 2008) |
| | Hyperactivity (6 DSM-5 symptoms) | DSM-5-TR A2a–f; ASRS; Vanderbilt; SNAP-IV |
| | Impulsivity (3 DSM-5 symptoms plus extras) | DSM-5-TR A2g–i; BAARS-IV; UPPS-P (Whiteside & Lynam 2001) |
| Executive function | Time management; Planning & organisation; Working memory; Task initiation & motivation | Barkley BDEFS five-factor model (2011); BRIEF-2 / BRIEF-A (Gioia 2015; Roth 2005); Brown EF/A (2019) |
| Emotion & arousal regulation | Emotional reactivity | Faraone et al. 2019; Shaw et al. 2014; DERS (Gratz & Roemer 2004); ARI (Stringaris 2012) |
| | Rejection sensitivity | RSQ (Downey & Feldman 1996); Children's RSQ (Downey 1998); Bondü & Esser 2015 |
| | Hyperfocus | Adult Hyperfocus Questionnaire (Hupfeld, Abagis & Shah 2019); Ashinoff & Abu-Akel 2021 |
| | Cognitive disengagement (formerly sluggish cognitive tempo) | Becker et al. 2016 meta-analysis; CDS consensus (Becker et al. 2023); Penny et al. 2009 |
| | Emotional awareness (alexithymia) | TAS-20 (Bagby 1994); Kinnaird et al. 2019 meta-analysis; AQC (Rieffe 2006) |
| Social communication & thinking style | Conversation & non-verbal; Social intuition; Routines & flexibility; Detail & pattern focus; Focused interests; Literal & imaginative thinking | AQ five subscales (Baron-Cohen 2001); AQ-Child (Auyeung 2008); RAADS-R (Ritvo 2011); SCQ; SRS-2; DSM-5 ASD A/B; weak central coherence (Happé & Frith 2006) |
| | Social camouflaging (adult only) | CAT-Q (Hull et al. 2019) |
| Sensory processing | Sensitivity & avoiding; Seeking & low registration | Dunn's quadrant model (1997); Adolescent/Adult Sensory Profile; Short Sensory Profile (McIntosh 1999); Glasgow Sensory Questionnaire (Robertson & Simmons 2013); DSM-5 ASD B4 |
| Learning & motor | Reading & spelling; Number processing; Motor coordination | ARHQ (Lefly & Pennington 2000); Vinegrad adult dyslexia checklist (1994); Colorado LDQ (Willcutt 2011); ADC (Kirby 2010); DCDQ'07 (Wilson 2009); DSM-5 SLD/DCD |
| Co-occurring behaviour (child only) | Oppositional behaviour | Vanderbilt ODD items; DSM-5. Shown separately and excluded from every index |

Full citations are listed in the page's **Method & sources** tab.

## Methodology

1. **Construct first.** Each dimension corresponds to a validated subscale or factor. Constructs with several facets are split, so executive function has 4 dimensions, sensory processing 2 and the autism spectrum 7 (6 on the child form).
2. **Every dimension has at least 3 items.** The range is 3 to 9. The ADHD items map one-to-one onto the 18 DSM-5 symptoms.
3. **Reverse-worded items**, as used in the AQ, reduce agreement bias. They are reverse-scored.
4. **Dimension score** = mean item score ÷ scale maximum × 100. The results show four bands anchored to the response labels: Typical range (0–29), Some traits (30–49), Elevated (50–69) and Pronounced (70–100).
5. **Cumulative indices** average their dimensions. There are six: ADHD core, Executive-function load, Regulation & arousal, Autism-spectrum, Learning & motor, and an Overall neurodivergence index. The overall index follows the transdiagnostic, dimensional view of neurodevelopmental conditions (Astle et al. 2022; Rommelse et al. 2010).
6. **Criterion screens** apply published counting rules:
   - DSM-5 symptom counts, where a symptom counts at "Often" or above. The threshold is 6 of 9 under age 17 and 5 of 9 from 17.
   - Adult form only: the ASRS v1.1 Part A rule, where 4 or more of the 6 items over their item-specific thresholds is screen-positive.

## Limits

- The scores are not normed, so they are not percentiles.
- Self-report and parent-report are affected by mood, insight and masking.
- Anxiety, depression, trauma, sleep problems, sensory impairment and medical conditions can all raise scores.
- A diagnosis requires a clinical assessment that covers developmental history, impairment in more than one setting and a differential diagnosis.

Answers are stored only in the viewer's browser (`localStorage`).
