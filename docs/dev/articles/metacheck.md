# metacheck

## Installation

You can install the development version of metacheck from
[GitHub](https://github.com/scienceverse/metacheck) with:

``` r
# install.packages("devtools")
devtools::install_github("scienceverse/metacheck")
```

``` r
library(metacheck)
```

You can launch an interactive shiny app version of the code below with:

``` r
metacheck_app()
```

### Load from PDF

The function
[`convert()`](https://scienceverse.github.io/metacheck/dev/reference/convert.md)
can read PDF files and save them in [JSON
format](https://www.scienceverse.org/schema/paper.json). This requires
an internet connection and takes a few seconds per paper, so should only
be done once and the results saved for later use.

``` r
pdf_file <- demofile("pdf")
json_file <- convert(file_path = pdf_file, save_path = "converted")
```

You can set up your own local grobid server following instructions from
<https://grobid.readthedocs.io/>. The easiest way is to use Docker.

``` bash
docker run --rm --init --ulimit core=0 -p 8070:8070 lfoppiano/grobid:0.9.0
```

Then you can set your api_url to the local path <http://localhost:8070>.

``` r
json_file <- convert(file_path = pdf_file, 
                     save_path = "converted",
                     method = "grobid",
                     api_url = "http://localhost:8070")
```

### Load from JSON

The function
[`read()`](https://scienceverse.github.io/metacheck/dev/reference/read.md)
can read converted JSON files.

``` r
paper <- read(json_file)
```

### Load from non-PDF document

To take advantage of grobid’s ability to parse references and other
aspects of papers, for now the best way is to convert your papers to
PDF. We will introduce our custom backend, bibr, soon and this will be
able to convert DOC and DOCX files directly.

### Batch Processing

The functions
[`convert()`](https://scienceverse.github.io/metacheck/dev/reference/convert.md)
and
[`read()`](https://scienceverse.github.io/metacheck/dev/reference/read.md)
also work on a folder of files, returning a list of JSON file paths or
paper objects, respectively. The functions
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md),
[`expand_text()`](https://scienceverse.github.io/metacheck/dev/reference/expand_text.md)
and
[`llm()`](https://scienceverse.github.io/metacheck/dev/reference/llm.md)
also work on a list of paper objects.

## Paper Components

Paper objects contain a lot of structured information, including info,
references, and citations.

### Info

``` r
paper$info
```

    #>                                         title     keywords  doi
    #> 1 To Err is Human: An Empirical Investigation list(list()) <NA>
    #>          file_hash input_format           file_name bibr_version paper_type
    #> 1 a26373a4f28e3718          pdf to_err_is_human.pdf         10.0  empirical
    #>   paper_type_confidence         oecd_l1                           oecd_l2
    #> 1                     0 Social Sciences Psychology and Cognitive Sciences
    #>   oecd_confidence
    #> 1              NA

### Bibliography

The bibliography is provided in a tabular format.

``` r
paper$bib
```

| bib_id | text_id | bib_type | doi | title | authors | editors | publisher | year | year_suffix | date | container | volume | issue | first_page | last_page | edition | version | url |
|---:|---:|:---|:---|:---|:---|:---|:---|---:|:---|:---|:---|:---|:---|:---|:---|:---|:---|:---|
| 1 | 29 | misc | 10.5281/zenodo.2669586 | Faux: Simulation for Factorial Designs | DeBruine, Lisa | NA | NA | 2025 | NA | NA |  | NA | NA | NA | NA | NA | NA | NA |
| 2 | 30 | article | 10.1037/0003-066x.54.6.408 | The Origins of Sex Differences in Human Behavior: Evolved Dispositions Versus Social Roles | Eagly, Alice H., and Wendy Wood | NA | NA | 1999 | NA | NA | American Psychologist | 54 | 6 | 408 | 423 | NA | NA | NA |
| 3 | 31 | article | 10.1177/0956797614520714 | Evil Genius? How Dishonesty Can Lead to Greater Creativity | Gino, Francesca, and Scott S. Wiltermuth | NA | NA | 2014 | NA | NA | Psychological Science | 25 | 4 | 973 | 981 | NA | NA | NA |
| 4 | 32 | article |  | Equivalence Testing for Psychological Research | Lakens, Daniël | NA | NA | 2018 | NA | NA | Advances in Methods and Practices in Psychological Science | 1 | NA | 259 | 270 | NA | NA | NA |
| 5 | 33 | article | 10.0000/0123456789 | Human Error Is a Symptom of a Poor Design | Smith, F. | NA | NA | 2021 | NA | NA | Journal of Journals | NA | NA | NA | NA | NA | NA | NA |

### Cross-References

Cross-references are also provided in a tabular format, with `xref_id`
to match the bibliography table.

``` r
paper$xref
```

| xref_id | xref_type | contents                   | text_id |
|--------:|:----------|:---------------------------|--------:|
|       1 | table     | Table 1                    |      20 |
|       1 | figure    | Figure 1                   |      20 |
|       2 | figure    | Figure 2                   |      23 |
|       1 | foot      | 1                          |      10 |
|       2 | foot      | 2                          |      19 |
|       3 | bib       | (Gino and Wiltermuth 2014) |       6 |
|      NA | bib       | (Smithy, 2020)             |       7 |
|       1 | bib       | (DeBruine 2025)            |      20 |

### Batch

There are functions to combine the infomation from a list of papers,
like the `psychsci` built-in dataset of 250 open access papers from
Psychological Science.

``` r
paper_table(psychsci[1:5], "info", c("title", "doi"))
```

    #>                                                                                                                                                                                                                              title
    #> 1 Mirror neurons, originally discovered in macaque monkeys using single-cell recordings, are active when an animal is either performing a particular action or observing another agent performing the same or a similar action (di
    #> 2                                                                                                                                         Beyond Gist: Strategic and Incremental Information Accumulation for Scene Categorization
    #> 3                                                                                      Serotonin and Social Norms: Tryptophan Depletion Impairs Social Comparison and Leads to Resource Depletion in a Multiplayer Harvesting Game
    #> 4                                                                                                                                                                              Action-Specific Disruption of Perceptual Confidence
    #> 5                                                                                                                                  Emotional Vocalizations Are Recognized Across Cultures Regardless of the Valence of Distractors
    #>                        doi         paper_id
    #> 1 10.1177/0956797613520608 0956797613520608
    #> 2 10.1177/0956797614522816 0956797614522816
    #> 3 10.1177/0956797614527830 0956797614527830
    #> 4 10.1177/0956797614557697 0956797614557697
    #> 5 10.1177/0956797614560771 0956797614560771

``` r
paper_table(psychsci[1:5], "bib") |>
  dplyr::filter(!is.na(doi))
```

    #>     bib_type                           doi
    #> 1    article                              
    #> 2    article                              
    #> 3    article                              
    #> 4    article                              
    #> 5    article                              
    #> 6    article                              
    #> 7    article                              
    #> 8    article                              
    #> 9    article                              
    #> 10   article                              
    #> 11   article                              
    #> 12   article                              
    #> 13   article                              
    #> 14   article                              
    #> 15   article                              
    #> 16   article                              
    #> 17   article                              
    #> 18   article                              
    #> 19   article                              
    #> 20   article                              
    #> 21   article                              
    #> 22   article                              
    #> 23   article                              
    #> 24   article                              
    #> 25   article                              
    #> 26   article                              
    #> 27   article                              
    #> 28   article                              
    #> 29   article                              
    #> 30   article                              
    #> 31   article                              
    #> 32   article                              
    #> 33   article                              
    #> 34   article                              
    #> 35   article                              
    #> 36   article                              
    #> 37   article                              
    #> 38   article                              
    #> 39   article                              
    #> 40   article                              
    #> 41   article                              
    #> 42   article                              
    #> 43   article                              
    #> 44   article                              
    #> 45   article                              
    #> 46   article                              
    #> 47   article                              
    #> 48   article                              
    #> 49   article                              
    #> 50   article                              
    #> 51   article                              
    #> 52   article                              
    #> 53   article                              
    #> 54   article                              
    #> 55   article                              
    #> 56   article                              
    #> 57   article                              
    #> 58   article                              
    #> 59   article                              
    #> 60   article                              
    #> 61   article                              
    #> 62   article                              
    #> 63   article                              
    #> 64   article                              
    #> 65   article                              
    #> 66   article                              
    #> 67   article                              
    #> 68   article                              
    #> 69   article                              
    #> 70   article                              
    #> 71   article                              
    #> 72   article                              
    #> 73   article                              
    #> 74   article                              
    #> 75   article                              
    #> 76   article                              
    #> 77   article                              
    #> 78   article                              
    #> 79   article                              
    #> 80   article                              
    #> 81   article                              
    #> 82   article                              
    #> 83   article                              
    #> 84   article                              
    #> 85   article                              
    #> 86   article                              
    #> 87   article                              
    #> 88   article                              
    #> 89   article                              
    #> 90   article                              
    #> 91   article                              
    #> 92   article                              
    #> 93   article                              
    #> 94   article                              
    #> 95   article                              
    #> 96   article                              
    #> 97   article                              
    #> 98   article                              
    #> 99   article                              
    #> 100  article                              
    #> 101  article                              
    #> 102  article                              
    #> 103  article                              
    #> 104  article                              
    #> 105  article                              
    #> 106  article                              
    #> 107  article                              
    #> 108  article                              
    #> 109  article                              
    #> 110  article                              
    #> 111  article                              
    #> 112  article                              
    #> 113  article                              
    #> 114  article                              
    #> 115  article                              
    #> 116  article                              
    #> 117  article                              
    #> 118  article                              
    #> 119  article                              
    #> 120  article                              
    #> 121  article                              
    #> 122  article                              
    #> 123  article                              
    #> 124  article                              
    #> 125  article                              
    #> 126  article                              
    #> 127  article                              
    #> 128  article                              
    #> 129  article                              
    #> 130  article                              
    #> 131  article                              
    #> 132  article                              
    #> 133  article                              
    #> 134  article                              
    #> 135  article                              
    #> 136  article                              
    #> 137  article                              
    #> 138  article                              
    #> 139  article                              
    #> 140  article                              
    #> 141  article                              
    #> 142  article                              
    #> 143  article                              
    #> 144  article                              
    #> 145  article                              
    #> 146  article 10.3389/fnint.2012.00079/full
    #> 147  article         10.1037/0033-2909.115
    #> 148  article      10.1177/0956797613517239
    #> 149  article   10.1037/0033-2909.115.1.102
    #> 150  article     10.1080/17470211003721642
    #> 151  article       10.1073/pnas.0908239106
    #>                                                                                                                                                          title
    #> 1                                                                                            Recovery from ideomotor apraxia: A study on acute stroke patients
    #> 2                                                              Action observation activates premotor and parietal areas in a somatotopic manner: An fMRI study
    #> 3                          On beyond mirror neurons: Internal representations subserving imitation and recognition of skilled object-related actions in humans
    #> 4                                   Statedependent TMS reveals a hierarchical representation of observed acts in the temporal, parietal, and premotor cortices
    #> 5                                                             Intracortical inhibition and facilitation in different representations of the human motor cortex
    #> 6                                                                                                                                                             
    #> 7                                                                                            Action mirroring and action understanding: An alternative account
    #> 8                                                                                                       Understanding motor events: A neurophysiological study
    #> 9                                                                                                              The role of social cognition in decision making
    #> 10                                                                                                                   Action recognition in the premotor cortex
    #> 11                                                                                                            A unifying view of the basis of social cognition
    #> 12       The observation and execution of actions share motor and somatosensory voxels in all tested subjects: Single-subject analyses of unsmoothed fMRI data
    #> 13                                       Functional organization of inferior area 6 in the macaque monkey: I. Somatotopy and the control of proximal movements
    #> 14             Depression of human corticospinal excitability induced by magnetic theta-burst stimulation: Evidence of rapid polarity-reversing metaplasticity
    #> 15                                                                                         Somatotopy of monkey premotor cortex examined with microstimulation
    #> 16                                                                                                The motor hierarchy: From kinematics to goals and intentions
    #> 17                                                                                 Somatotopic representation of action words in the motor and premotor cortex
    #> 18                                                                   Eight problems for the mirror neuron theory of action understanding in monkeys and humans
    #> 19                                                                                                           Theta burst stimulation of the human motor cortex
    #> 20                                   The effect of continuous theta burst stimulation over premotor cortex on circuits in primary motor cortex and spinal cord
    #> 21                                                                                       Grasping the intentions of others with one's own mirror neuron system
    #> 22                                                   Coding observed motor acts: Different organizational principles in parietal and premotor cortex of humans
    #> 23                                                                                                   Predictive coding: An account of the mirror neuron system
    #> 24                                                                                                  Evidence of mirror neurons in human inferior frontal gyrus
    #> 25                                              Charting the excitability of premotor to motor connections while withholding or initiating a selected movement
    #> 26                                                                                                The elusive lesion-apraxia of speech link in Broca's aphasia
    #> 27                                                                                                       The neural basis of body form and body action agnosia
    #> 28                                                                               Single-neuron responses in humans during execution and observation of actions
    #> 29                                       Functional connectivity of human premotor and motor cortex explored with repetitive transcranial magnetic stimulation
    #> 30                                                                                Neural underpinnings of gesture discrimination in patients with limb apraxia
    #> 31                                                                                              Action understanding requires the left inferior frontal cortex
    #> 32                                                          Progressive ideomotor apraxia: Evidence for a selective impairment of the action production system
    #> 33                                           Functional organization of inferior area 6 in the macaque monkey: II. Area F5 and the control of distal movements
    #> 34                                                                                                                                    The mirror-neuron system
    #> 35                                                                                                        Premotor cortex and the recognition of motor actions
    #> 36                                                          Action comprehension in aphasia: Linguistic and nonlinguistic deficits and their lesion correlates
    #> 37                                                                                                                                                            
    #> 38                                                                 Functional-anatomical concepts of human premotor cortex: Evidence from fMRI and PET studies
    #> 39                                                                                     Action simulation plays a critical role in deceptive action recognition
    #> 40                                                      Representation of body identity and body actions in extrastriate body area and ventral premotor cortex
    #> 41                                                             Prefrontal involvement in imitation learning of hand actions: Effects of practice and expertise
    #> 42                                                                                            Recognition-by-components: A theory of human image understanding
    #> 43                                                                                                                                   Visual object recognition
    #> 44                                                     Ultrarapid categorisation of natural scenes does not rely on colour cues: A study in monkeys and humans
    #> 45                                                                                                       What do we perceive in a glance of a real-world scene
    #> 46                                                                       Why do we SLIP to the basic level? Computational constraints and their implementation
    #> 47                                                                                     The briefest of glances: The time course of natural scene understanding
    #> 48                                                      Recognition of natural scenes from global properties: Seeing the forest without representing the trees
    #> 49                                                                                       Processing scene context: Fast categorization and object interference
    #> 50                                                         The natural/man-made distinction is made prior to basic-level distinctions in scene gist processing
    #> 51                                                                                             The span of the effective stimulus during a fixation in reading
    #> 52                                        Coarse blobs or fine edges? Evidence that information diagnosticity changes the perception of complex visual stimuli
    #> 53                                                                          Modeling the shape of the scene: A holistic representation of the spatial envelope
    #> 54                                                                              Building the gist of a scene: The role of global image features in recognition
    #> 55                                                                                                                                    Meaning in visual scenes
    #> 56                                                                                                                   Short-term conceptual memory for pictures
    #> 57                                                                                                                                Principles of categorization
    #> 58                                                                                                                         Basic objects in natural categories
    #> 59                                                         Effects of spatial frequency bands on perceptual decision: It is not the stimuli but the comparison
    #> 60                                                                                                 How long to get to the "gist" of real-world natural scenes?
    #> 61                                                                        Diagnostic recognition: Task constraints, object information, and their interactions
    #> 62                                                               From blobs to boundary edges: Evidence for time-and spatial-scale-dependent scene recognition
    #> 63                                                                                                A feedforward architecture accounts for rapid categorization
    #> 64                                                                                                              Speed of processing in the human visual system
    #> 65                                                                                                                      Statistics of natural image categories
    #> 66                                                                                                                          Categories of environmental scenes
    #> 67                                                                                                                Surfing a spike wave down the ventral stream
    #> 68                                                                         Trustworthiness and competitive altruism can also solve the "tragedy of the commons
    #> 69                     Activation of social norms in social dilemmas: A review of the evidence and reflections on the implications for environmental behaviour
    #> 70                                                                                                                                               Van der Kloot
    #> 71                                                                                  Money and happiness: Rank of income, not income, affects life satisfaction
    #> 72                                                                                Hardin revisited: A critical look at perception and the logic of the commons
    #> 73                                                                        Understanding the social costs of narcissism: The case of the tragedy of the commons
    #> 74                                                                                                          Committee on the Human Dimensions of Global Change
    #> 75                                                                                Serotonin modulates striatal responses to fairness and retaliation in humans
    #> 76                                                                                                      Serotonin modulates behavioral reactions to unfairness
    #> 77                                                                           Influence of trait hostility on tryptophan depletioninduced laboratory aggression
    #> 78                                                                                                          The tragedy of the commons: Twenty-two years later
    #> 79                                                                                                                             Altruistic punishment in humans
    #> 80                                                                                                                                                            
    #> 81                                                                     Social preferences, beliefs, and the dynamics of free riding in public good experiments
    #> 82                                                                               Are people conditionally cooperative? Evidence from a public goods experiment
    #> 83                                                                                                                             The real tragedy of the commons
    #> 84                                                                                                                                  The tragedy of the commons
    #> 85                                                               How uncertainty stimulates over-harvesting in a resource dilemma: Three possible explanations
    #> 86                                                                        Selective alteration of personality and social behavior by serotonergic intervention
    #> 87                                                   Irrational economic decisionmaking after ventromedial prefrontal damage: Evidence from the ultimatum game
    #> 88                                                                                                                 Social dilemmas: The anatomy of cooperation
    #> 89                                                Am I abnormal? Relative rank and social norm effects in judgments of anxiety and depression symptom severity
    #> 90                                                                                       Clinical and physiological consequences of rapid tryptophan depletion
    #> 91                                                                                                                 Normative social influence is underdetected
    #> 92                                                                                                                                   Reformulating the commons
    #> 93                                                                               A general framework for analyzing sustainability of social-ecological systems
    #> 94                                                                                                         Factor structure of the Barratt Impulsiveness Scale
    #> 95                                                                          Serotonergic mechanisms promote dominance acquisition in adult male vervet monkeys
    #> 96                                                                                                          The tragedy of the commons in evolutionary biology
    #> 97                                                                                                                                                            
    #> 98                       Mood is indirectly related to serotonin, norepinephrine and dopamine levels in humans: A meta-analysis of monoamine depletion studies
    #> 99                                                                                          Low-serotonin levels increase delayed reward discounting in humans
    #> 100                                            Serotonin differentially regulates short-and long-term prediction of rewards in the ventral and dorsal striatum
    #> 101                                                                                                   Social norms and cooperation in reallife social dilemmas
    #> 102                                                             Development and validation of brief measures of positive and negative affect: The PANAS scales
    #> 103                          An evolutionary based social rank explanation of why low income predicts mental distress: A 17 year cohort study of 30,000 people
    #> 104                                                Effects of tryptophan depletion on the performance of an iterated prisoner's dilemma game in healthy adults
    #> 105                                                                 Psychological traces of China's socio-economic reforms in the ultimatum and dictator games
    #> 106                                                                                           Flexible mechanisms underlie the evaluation of visual confidence
    #> 107                                                                                                                                                           
    #> 108                                                                                     BOLD MRI responses to repetitive TMS over human dorsal premotor cortex
    #> 109                                                                                                                                                           
    #> 110             Low-frequency rTMS over lateral premotor cortex induces lasting changes in regional activation and functional coupling of cortical motor areas
    #> 111               Neural correlates of reaching decisions in dorsal premotor cortex: Specification of multiple direction choices and final selection of action
    #> 112                                                                                                                                                           
    #> 113                                                                            Simultaneous over-and under-confidence: The role of error in judgment processes
    #> 114                                                                                    Prefrontal contributions to metacognition in perceptual decision making
    #> 115                                                                               Relating introspective accuracy to individual differences in brain structure
    #> 116                                                                                                                                                           
    #> 117                                                                             A proposed common neural mechanism for categorization and perceptual decisions
    #> 118                                                 Type 2 tasks in the theory of signal detectability: Discrimination between correct and incorrect decisions
    #> 119                                                                                                                        The neural basis of decision making
    #> 120                                                                                                                 The cortical control of movement revisited
    #> 121                                                                           Premotor cortex and the conditions for movement in monkeys (Macaca fascicularis)
    #> 122                                                                                  Temporal evolution of a decision-making process in medial premotor cortex
    #> 123                                                                                      The role of ipsilateral premotor cortex in hand movement after stroke
    #> 124                                                                               Neural correlates, computation and behavioural impact of decision confidence
    #> 125                                                                  Representation of confidence associated with a decision by neurons in the parietal cortex
    #> 126                                                     A signal detection theoretic approach for estimating metacognitive sensitivity from confidence ratings
    #> 127                                                                                         An application of the Poisson race model to confidence calibration
    #> 128                                                                            Coil placement in magnetic brain stimulation related to skull and brain anatomy
    #> 129                                                                            Corticomotor threshold to magnetic stimulation: Normal values and repeatability
    #> 130                                                                                                       Visual receptive fields of frontal eye field neurons
    #> 131                                                                                                                                 The secret life of fluency
    #> 132                                                               Functional specificity of human premotor-motor cortical interactions during action selection
    #> 133                                                                                                                                                           
    #> 134                                                                       The VideoToolbox software for visual psychophysics: Transforming numbers into movies
    #> 135                                                                      Two-stage dynamic signal detection: A theory of choice, decision time, and confidence
    #> 136                     Localization of the human frontal eye fields and motor hand area with transcranial magnetic stimulation and magnetic resonance imaging
    #> 137                                                                               Locating the human frontal eye fields with transcranial magnetic stimulation
    #> 138                                                                                    Neuronal correlates of a perceptual decision in ventral premotor cortex
    #> 139                                              Theta-burst transcranial magnetic stimulation to the prefrontal cortex impairs metacognitive visual awareness
    #> 140 Temporary interference in human lateral premotor cortex suggests dominance for the selection of movements. A study using transcranial magnetic stimulation
    #> 141                                                               Neural basis of a perceptual decision in the parietal cortex (area LIP) of the rhesus monkey
    #> 142                                                                                                                                                           
    #> 143                                                                                                             QUEST: A Bayesian adaptive psychometric method
    #> 144                                                                           Subliminal priming of actions influences sense of control over effects of action
    #> 145                                                                                    Metacognition in human decision-making: Confidence and error monitoring
    #> 146                                                                                                    The construction of confidence in a perceptual decision
    #> 147                                                               Strong evidence for universals in facial expressions: A reply to Russell's mistaken critique
    #> 148                                                                                               Cultural relativity in perceiving emotion from vocalizations
    #> 149                                                   Is there universal recognition of emotion from facial expression? A review of the cross-cultural studies
    #> 150                                                                                                 Perceptual cues in non-verbal vocal expressions of emotion
    #> 151                                                                      Crosscultural recognition of basic emotions through nonverbal emotional vocalizations
    #>                                                                                             authors
    #> 1                                    Basso, A; Capitani, E; Della Sala, S; Laiacona, M; Spinnler, H
    #> 2        Buccino, G; Binkofski, F; Fink, G R; Fadiga, L; Fogassi, L; Gallese, V; Freund, . .; , H J
    #> 3                                                                 Buxbaum, L J; Kyle, K M; Menon, R
    #> 4                                                          Cattaneo, L; Sandrini, M; Schwarzbach, J
    #> 5                  Chen, R; Tam, A; Butefisch, C; Corwell, B; Ziemann, U; Rothwell, J C; Cohen, L G
    #> 6                                                 Cook, R; Bird, G; Catmur, C; Press, C; Heyes, C M
    #> 7                                                                                         Csibra, G
    #> 8                                Di Pellegrino, G; Fadiga, L; Fogassi, L; Gallese, V; Rizzolatti, G
    #> 9                                                                             Frith, C D; Singer, T
    #> 10                                                 Gallese, V; Fadiga, L; Fogassi, L; Rizzolatti, G
    #> 11                                                            Gallese, V; Keysers, C; Rizzolatti, G
    #> 12                                                                           Gazzola, V; Keysers, C
    #> 13                     Gentilucci, M; Fogassi, L; Luppino, G; Matelli, M; Camarda, R; Rizzolatti, G
    #> 14                                    Gentner, R; Wankerl, K; Reinsberger, C; Zeller, D; Classen, J
    #> 15                                            Godschalk, M; Mitz, A R; Van Duin, B; Van Der Burg, H
    #> 16                                                                          Hamilton, A; Grafton, S
    #> 17                                                           Hauk, O; Johnsrude, I; Pulvermüller, F
    #> 18                                                                                        Hickok, G
    #> 19                                  Huang, Y Z; Edwards, M J; Rounis, E; Bhatia, K P; Rothwell, J C
    #> 20                      Huang, Y Z; Rothwell, J C; Lu, C S; Wang, J; Weng, Y H; Lai, S C; Chen, R S
    #> 21            Iacoboni, M; Molnar-Szakacs, I; Gallese, V; Buccino, G; Mazziotta, J C; Rizzolatti, G
    #> 22                            Jastorff, J; Begliomini, C; Fabbri-Destro, M; Rizzolatti, G; Orban, G
    #> 23                                                                  Kilner, J; Friston, K; Frith, C
    #> 24                                            Kilner, J; Neal, A; Weiskopf, N; Friston, K; Frith, C
    #> 25                             Kroeger, J; Bäumer, T; Jonas, M; Rothwell, J; Siebner, H; Münchau, A
    #> 26                                                                         Marquardt, T; Sussman, H
    #> 27                           Moro, V; Urgesi, C; Pernigo, S; Lanteri, P; Pazzaglia, M; Aglioti, S M
    #> 28                                       Mukamel, R; Ekstrom, A D; Kaplan, J; Iacoboni, M; Fried, I
    #> 29                                Münchau, A; Bloem, B R; Irlbacher, K; Trimble, M R; Rothwell, J C
    #> 30                                                 Pazzaglia, M; Smania, N; Corato, E; Aglioti, S M
    #> 31                                                                           Pobric, G; Hamilton, A
    #> 32                                               Rapcsak, S Z; Ochipa, C; Anderson, K C; Poizner, H
    #> 33                     Rizzolatti, G; Camarda, R; Fogassi, L; Gentilucci, M; Luppino, G; Matelli, M
    #> 34                                                                      Rizzolatti, G; Craighero, L
    #> 35                                                 Rizzolatti, G; Fadiga, L; Gallese, V; Fogassi, L
    #> 36                                                Saygin, A P; Wilson, S M; Dronkers, N F; Bates, E
    #> 37                                                          Schneider, W; Eschman, A; Zuccolotto, A
    #> 38                                                                       Schubotz, R; Von Cramon, Y
    #> 39                                         Tidoni, E; Borgomaneri, S; Di Pellegrino, G; Avenanti, A
    #> 40                                                      Urgesi, C; Candidi, M; Ionta, S; Aglioti, S
    #> 41       Vogt, S; Buccino, G; Wohlschlager, A M; Canessa, N; Shah, N J; Zilles, K; Fink, . .; , G R
    #> 42                                                                                     Biederman, I
    #> 43                                                                                     Biederman, I
    #> 44                                                          Delorme, A; Richard, G; Fabre-Thorpe, M
    #> 45                                                          Fei-Fei, L; Iyer, A; Koch, C; Perona, P
    #> 46                                                                         Gosselin, F; Schyns, P G
    #> 47                                                                            Greene, M R; Oliva, A
    #> 48                                                                            Greene, M R; Oliva, A
    #> 49                                               Joubert, O; Rousselet, G; Fize, D; Fabre-Thorpe, M
    #> 50                                                                        Loschky, L C; Larson, A M
    #> 51                                                                         Mcconkie, G W; Rayner, K
    #> 52                                                                              Oliva, A; Schyns, P
    #> 53                                                                            Oliva, A; Torralba, A
    #> 54                                                                            Oliva, A; Torralba, A
    #> 55                                                                                      Potter, M C
    #> 56                                                                                      Potter, M C
    #> 57                                                                                         Rosch, E
    #> 58                                   Rosch, E; Mervis, C B; Gray, W D; Johnson, D M; Boyes-Braem, P
    #> 59                                           Rotshtein, P; Schofield, A; Funes, M J; Humphreys, G W
    #> 60                                                    Rousselet, G A; Joubert, O R; Fabre-Thorpe, M
    #> 61                                                                                      Schyns, P G
    #> 62                                                                            Schyns, P G; Oliva, A
    #> 63                                                                    Serre, T; Oliva, A; Poggio, T
    #> 64                                                                  Thorpe, S J; Fize, D; Marlot, C
    #> 65                                                                            Torralba, A; Oliva, A
    #> 66                                                                          Tversky, B; Hemenway, K
    #> 67                                                                        Vanrullen, R; Thorpe, S J
    #> 68                                                                                       Barclay, P
    #> 69                                                                            Biel, A; Thøgersen, J
    #> 70                       Booij, L; Van Der Does, W; Benkelfat, C; Bremner, J D; Cowen, P J; Fava, M
    #> 71                                                             Boyce, C J; Brown, G D A; Moore, S C
    #> 72                                                                                       Burke, B E
    #> 73                                               Campbell, W K; Bush, C P; Brunell, A B; Shelton, J
    #> 74                                                                                                 
    #> 75  Crockett, M J; Apergis-Schoute, A; Herrmann, B; Lieberman, M; Müller, U; Robbins, T W; Clark, L
    #> 76                               Crockett, M J; Clark, L; Tabibnia, G; Lieberman, M D; Robbins, T W
    #> 77                                             Dougherty, D M; Bjork, J M; Marsh, D M; Moeller, F G
    #> 78                                                    Feeny, D; Berkes, F; Mccay, B J; Acheson, J M
    #> 79                                                                              Fehr, E; Gächter, S
    #> 80                                             First, M B; Spitzer, R L; Gibbon, M; Williams, J B W
    #> 81                                                                       Fischbacher, U; Gächter, S
    #> 82                                                              Fischbacher, U; Gächter, S; Fehr, E
    #> 83                                                                                    Gardiner, S M
    #> 84                                                                                        Hardin, G
    #> 85                                                              Jager, W; Janssen, M A; Vlek, C A J
    #> 86       Knutson, B; Wolkowitz, O M; Cole, S W; Chan, T; Moore, E A; Johnson, R C; Reus, . .; , V I
    #> 87                                                                            Koenigs, M; Tranel, D
    #> 88                                                                                       Kollock, P
    #> 89                                                            Melrose, K L; Brown, G D A; Wood, A M
    #> 90          Moore, P; Landolt, H P; Seifritz, E; Clark, C; Bhatti, T; Kelsoe, J; Gillin, . .; , J C
    #> 91                         Nolan, J M; Schultz, P W; Cialdini, R B; Goldstein, N J; Griskevicius, V
    #> 92                                                                                        Ostrom, E
    #> 93                                                                                        Ostrom, E
    #> 94                                                         Patton, J H; Stanford, M S; Barratt, E S
    #> 95                               Raleigh, M J; Mcguire, M T; Brammer, G L; Pollack, D B; Yuwiler, A
    #> 96                                                                 Rankin, D J; Bargum, K; Kokko, H
    #> 97                                                    Raven, J; , B; , C; , D; Oxford, E; England, 
    #> 98                                                               Ruhe, H G; Mason, N S; Schene, A H
    #> 99           Schweighofer, N; Bertin, M; Shishida, K; Okamoto, Y; Tanaka, S C; Yamawaki, S; Doya, K
    #> 100           Tanaka, S C; Schweighofer, N; Asahi, S; Shishida, K; Okamoto, Y; Yamawaki, S; Doya, K
    #> 101                                                                                    Thøgersen, J
    #> 102                                                              Watson, D; Clark, L A; Tellegen, A
    #> 103                                                 Wood, A M; Boyce, C J; Moore, S C; Brown, G D A
    #> 104                                 Wood, R M; Rilling, J K; Sanfey, A G; Bhagwagar, Z; Rogers, R D
    #> 105                                                               Zhu, L; Gigerenzer, G; Huangfu, G
    #> 106                                                                      Barthelme, S; Mamassian, P
    #> 107                                                     Bates, D; Maechler, M; Bolker, B; Walker, S
    #> 108                                 Bestmann, S; Baudewig, J; Siebner, H R; Rothwell, J C; Frahm, J
    #> 109                                                                                   Brainard, D H
    #> 110                                               Chen, W H; Mima, T; Siebner, H R; Oga, T; Hara, H
    #> 111                                                                          Cisek, P; Kalaska, J F
    #> 112                                                                         Efron, B; Tibshirani, R
    #> 113                                                            Erev, I; Wallsten, T S; Budescu, D V
    #> 114                                                            Fleming, S M; Huijgen, J; Dolan, R J
    #> 115                                           Fleming, S M; Weil, R S; Nagy, Z; Dolan, R J; Rees, G
    #> 116                                                                             Fox, J; Weisberg, S
    #> 117                                                                       Freedman, D J; Assad, J A
    #> 118                                                    Galvin, S J; Podd, J V; Drga, V; Whitmore, J
    #> 119                                                                         Gold, J I; Shadlen, M N
    #> 120                                            Graziano, M S A; Taylor, C S R; Moore, T; Cooke, D F
    #> 121                                                                    Halsband, U; Passingham, R E
    #> 122                                                                Hernandez, A; Zainos, A; Romo, R
    #> 123  Johansen-Berg, H; Rushworth, M F S; Bogdanovic, M D; Kischka, U; Wimalaratna, S; Matthews, P M
    #> 124                                                Kepecs, A; Uchida, N; Zariwala, H A; Mainen, Z F
    #> 125                                                                          Kiani, R; Shadlen, M N
    #> 126                                                                           Maniscalco, B; Lau, H
    #> 127                                                                       Merkle, E C; Van Zandt, T
    #> 128                                               Meyer, B U; Britton, T C; Kloten, H; Steinmetz, H
    #> 129                                                                          Mills, K R; Nithi, K A
    #> 130                                                          Mohler, C W; Goldberg, M E; Wurtz, R H
    #> 131                                                                                Oppenheimer, D M
    #> 132                       O'shea, J; Sebastian, C; Boorman, E D; Johansen-Berg, H; Rushworth, M F S
    #> 133                                                                                 Passingham, R E
    #> 134                                                                                      Pelli, D G
    #> 135                                                                    Pleskac, T J; Busemeyer, J R
    #> 136                                                 Ro, T; Cheifet, S; Ingle, H; Shoup, R; Rafal, R
    #> 137                                                                       Ro, T; Farnè, A; Chang, E
    #> 138                                                                Romo, R; Hernandez, A; Zainos, A
    #> 139                                    Rounis, E; Maniscalco, B; Rothwell, J; Passingham, R; Lau, H
    #> 140                                      Schluter, N D; Rushworth, M F; Passingham, R E; Mills, K R
    #> 141                                                                      Shadlen, M N; Newsome, W T
    #> 142                                                                                      Vickers, D
    #> 143                                                                         Watson, A B; Pelli, D G
    #> 144                                                              Wenke, D; Fleming, S M; Haggard, P
    #> 145                                                                        Yeung, N; Summerfield, C
    #> 146                                                          Zylberberg, A; Barttfeld, P; Sigman, M
    #> 147                                                                                        Ekman, P
    #> 148                                       Gendron, M; Roberson, D; Van Der Vyver, J M; Barrett, L F
    #> 149                                                                                    Russell, J A
    #> 150                                                 Sauter, D A; Eisner, F; Calder, A J; Scott, S K
    #> 151                                                    Sauter, D A; Eisner, F; Ekman, P; Scott, S K
    #>                                        editors                  publisher year
    #> 1                                                                         1987
    #> 2                                                                         2001
    #> 3                                                                         2005
    #> 4                                                                         2010
    #> 5                                                                         1998
    #> 6                                                                           NA
    #> 7           Haggard, P; Rossetti, Y; Kawato, M    Oxford University Press 2008
    #> 8                                                                         1992
    #> 9                                                                         2008
    #> 10                                                                        1996
    #> 11                                                                        2004
    #> 12                                                                        2009
    #> 13                                                                        1988
    #> 14                                                                        2008
    #> 15                                                                        1995
    #> 16          Haggard, P; Rossetti, Y; Kawato, M    Oxford University Press 2007
    #> 17                                                                        2004
    #> 18                                                                        2009
    #> 19                                                                        2005
    #> 20                                                                        2009
    #> 21                                                                        2005
    #> 22                                                                        2010
    #> 23                                                                        2007
    #> 24                                                                        2009
    #> 25                                                                        2010
    #> 26    Rosenbek, J C; Mcneil, M R; Aronson, A E         College-Hill Press 1984
    #> 27                                                                        2008
    #> 28                                                                        2010
    #> 29                                                                        2002
    #> 30                                                                        2008
    #> 31                                                                        2006
    #> 32                                                                        1995
    #> 33                                                                        1988
    #> 34                                                                        2004
    #> 35                                                                        1996
    #> 36                                                                        2004
    #> 37                                              Psychology Software Tools 2001
    #> 38                                                                        2003
    #> 39                                                                        2013
    #> 40                                                                        2006
    #> 41                                                                        2007
    #> 42                                                                        1987
    #> 43                 Kosslyn, S F; Osherson, D N                  MIT Press 1995
    #> 44                                                                        2000
    #> 45                                                                        2007
    #> 46                                                                        2001
    #> 47                                                                        2009
    #> 48                                                                        2009
    #> 49                                                                        2007
    #> 50                                                                        2010
    #> 51                                                                        1975
    #> 52                                                                        1997
    #> 53                                                                        2001
    #> 54                                                                        2006
    #> 55                                                                        1975
    #> 56                                                                        1976
    #> 57                        Rosch, E; Lloyd, B B                    Erlbaum 1978
    #> 58                                                                        1976
    #> 59                                                                        2010
    #> 60                                                                        2005
    #> 61                                                                        1998
    #> 62                                                                        1994
    #> 63                                                                        2007
    #> 64                                                                        1996
    #> 65                                                                        2003
    #> 66                                                                        1983
    #> 67                                                                        2002
    #> 68                                                                        2004
    #> 69                                                                        2007
    #> 70                                                                        2002
    #> 71                                                                        2010
    #> 72                                                                        2001
    #> 73                                                                        2005
    #> 74  Ostrom, E; Dietz, T; Dolsak, N; Stern, P C   National Academies Press 2002
    #> 75                                                                        2013
    #> 76                                                                        2008
    #> 77                                                                        1999
    #> 78                                                                        1990
    #> 79                                                                        2002
    #> 80                                                    Biometrics Research 2002
    #> 81                                                                        2010
    #> 82                                                                        2001
    #> 83                                                                        2001
    #> 84                                                                        1968
    #> 85                                                                        2002
    #> 86                                                                        1998
    #> 87                                                                        2007
    #> 88                                                                        1998
    #> 89                                                                        2012
    #> 90                                                                        2000
    #> 91                                                                        2008
    #> 92                                                                        2000
    #> 93                                                                        2009
    #> 94                                                                        1995
    #> 95                                                                        1991
    #> 96                                                                        2007
    #> 97                                             Oxford Psychologists Press 1996
    #> 98                                                                        2007
    #> 99                                                                        2008
    #> 100                                                                       2007
    #> 101                                                                       2008
    #> 102                                                                       1988
    #> 103                                                                       2012
    #> 104                                                                       2006
    #> 105                                                                       2013
    #> 106                                                                       2010
    #> 107                                                                       2014
    #> 108                                                                       2005
    #> 109                                                                       1997
    #> 110                                                                       2003
    #> 111                                                                       2005
    #> 112                                                             CRC Press 1993
    #> 113                                                                       1994
    #> 114                                                                       2012
    #> 115                                                                       2010
    #> 116                                                                  Sage 2011
    #> 117                                                                       2011
    #> 118                                                                       2003
    #> 119                                                                       2007
    #> 120                                                                       2002
    #> 121                                                                       1985
    #> 122                                                                       2002
    #> 123                                                                       2002
    #> 124                                                                       2008
    #> 125                                                                       2009
    #> 126                                                                       2012
    #> 127                                                                       2006
    #> 128                                                                       1991
    #> 129                                                                       1997
    #> 130                                                                       1973
    #> 131                                                                       2008
    #> 132                                                                       2007
    #> 133                                               Oxford University Press 1993
    #> 134                                                                       1997
    #> 135                                                                       2010
    #> 136                                                                       1999
    #> 137                                                                       2002
    #> 138                                                                       2004
    #> 139                                                                       2010
    #> 140                                                                       1998
    #> 141                                                                       2001
    #> 142                                                        Academic Press 1979
    #> 143                                                                       1983
    #> 144                                                                       2010
    #> 145                                                                       2012
    #> 146                                                                       2012
    #> 147                                                                       1994
    #> 148                                                                       2014
    #> 149                                                                       1994
    #> 150                                                                       2010
    #> 151                                                                       2010
    #>     volume issue first_page last_page
    #> 1      110              747       760
    #> 2       13              400       404
    #> 3       25              226       239
    #> 4       20             2252      2258
    #> 5       80             2870      2881
    #> 6                      <NA>      <NA>
    #> 7                       435       458
    #> 8       91              176       180
    #> 9      363             3875      3886
    #> 10     119              593       609
    #> 11       8              396       403
    #> 12      19             1239      1255
    #> 13      71              475       490
    #> 14      18             2046      2053
    #> 15      23              269       279
    #> 16                      381       408
    #> 17      41              301       307
    #> 18      21             1229      1243
    #> 19      45              201       206
    #> 20     120              796       801
    #> 21       3              529       535
    #> 22     104              128       140
    #> 23       8              159       166
    #> 24      29            10153     10159
    #> 25      32             1771      1779
    #> 26                       91       112
    #> 27      60              235       246
    #> 28      20              750       756
    #> 29      22              554       561
    #> 30      28             3030      3041
    #> 31      16              524       529
    #> 32      27              213       236
    #> 33      71              491       507
    #> 34      27              169       192
    #> 35       3              131       141
    #> 36      42             1788      1804
    #> 37                     <NA>      <NA>
    #> 38      20              120      S131
    #> 39      33              611       623
    #> 40      10               30        31
    #> 41      37             1371      1383
    #> 42      94              115       147
    #> 43       2              121       165
    #> 44      40             2187      2200
    #> 45       7     1       <NA>      <NA>
    #> 46     108              735       758
    #> 47      20              464       472
    #> 48      58              137       179
    #> 49      47             3286      3297
    #> 50      18              513       536
    #> 51      17              578       586
    #> 52      34               72       107
    #> 53      42              145       175
    #> 54     155               23        36
    #> 55     187              965       966
    #> 56       2              509       522
    #> 57                       28        48
    #> 58       8              382       439
    #> 59      10    10       <NA>      <NA>
    #> 60      12              852       877
    #> 61      67              147       179
    #> 62       5              195       200
    #> 63     104             6424      6429
    #> 64     381              520       522
    #> 65      14              391       412
    #> 66      15              121       149
    #> 67      42             2593      2615
    #> 68      25              209       220
    #> 69      28               93       112
    #> 70      27              852       861
    #> 71      21              471       475
    #> 72      29              449       476
    #> 73      31             1358      1368
    #> 74                     <NA>      <NA>
    #> 75      33             3505      3513
    #> 76     320             1739      <NA>
    #> 77      88              227       232
    #> 78      18                1        19
    #> 79     415              137       140
    #> 80                     <NA>      <NA>
    #> 81     100              541       556
    #> 82      71              397       404
    #> 83      30              387       416
    #> 84     162             1243      1248
    #> 85      22              247       263
    #> 86     155              373       379
    #> 87      27              951       956
    #> 88      24              183       214
    #> 89      26              174       184
    #> 90      23              601       622
    #> 91      34              913       923
    #> 92       6     1         29        52
    #> 93     325              419       422
    #> 94      51              768       774
    #> 95     559              181       190
    #> 96      22              643       651
    #> 97                     <NA>      <NA>
    #> 98      12              331       359
    #> 99      28             4528      4532
    #> 100      2    12       <NA>      <NA>
    #> 101     29              458       472
    #> 102     54             1063      1070
    #> 103    136              882       888
    #> 104     31             1075      1084
    #> 105      8     8       <NA>      <NA>
    #> 106    107            20834     20839
    #> 107                       1         7
    #> 108     28               22        29
    #> 109     10              433       436
    #> 110    114             1628      1637
    #> 111     45              801       814
    #> 112                    <NA>      <NA>
    #> 113    101              519       527
    #> 114     32             6117      6125
    #> 115    329             1541      1543
    #> 116                    <NA>      <NA>
    #> 117     14              143       146
    #> 118     10              843       876
    #> 119     30              535       574
    #> 120     36              349       362
    #> 121     18              269       277
    #> 122     33              959       972
    #> 123     99            14518     14523
    #> 124    455              227       231
    #> 125    324              759       764
    #> 126     21              422       430
    #> 127    135              391       408
    #> 128     81               38        46
    #> 129     20              570       576
    #> 130     61              385       389
    #> 131     12              237       241
    #> 132     26             2085      2095
    #> 133                    <NA>      <NA>
    #> 134     10              437       442
    #> 135    117              864       901
    #> 136     37              225       231
    #> 137     24              930       940
    #> 138     41              165       173
    #> 139      1              165       175
    #> 140    121              785       799
    #> 141     86             1916      1936
    #> 142                    <NA>      <NA>
    #> 143     33              113       120
    #> 144    115               26        38
    #> 145    367             1310      1321
    #> 146      6             <NA>      <NA>
    #> 147    115              268       287
    #> 148     25              911       920
    #> 149    115              102       141
    #> 150     63             2251      2272
    #> 151    107             2408      2412
    #>                                                                  container
    #> 1                                                                    Brain
    #> 2                                         European Journal of Neuroscience
    #> 3                                                 Cognitive Brain Research
    #> 4                                                          Cerebral Cortex
    #> 5                                               Journal of Neurophysiology
    #> 6                                                                         
    #> 7                                                                         
    #> 8                                              Experimental Brain Research
    #> 9   Philosophical Transactions of the Royal Society B: Biological Sciences
    #> 10                                                                   Brain
    #> 11                                            Trends in Cognitive Sciences
    #> 12                                                         Cerebral Cortex
    #> 13                                             Experimental Brain Research
    #> 14                                                         Cerebral Cortex
    #> 15                                                   Neuroscience Research
    #> 16                                                                        
    #> 17                                                                  Neuron
    #> 18                                       Journal of Cognitive Neuroscience
    #> 19                                                                  Neuron
    #> 20                                                Clinical Neurophysiology
    #> 21                                                            PLoS Biology
    #> 22                                              Journal of Neurophysiology
    #> 23                                                    Cognitive Processing
    #> 24                                             The Journal of Neuroscience
    #> 25                                        European Journal of Neuroscience
    #> 26                                                                        
    #> 27                                                                  Neuron
    #> 28                                                         Current Biology
    #> 29                                             The Journal of Neuroscience
    #> 30                                             The Journal of Neuroscience
    #> 31                                                         Current Biology
    #> 32                                                     Brain and Cognition
    #> 33                                             Experimental Brain Research
    #> 34                                           Annual Review of Neuroscience
    #> 35                                                Cognitive Brain Research
    #> 36                                                        Neuropsychologia
    #> 37                                                                        
    #> 38                                                              NeuroImage
    #> 39                                             The Journal of Neuroscience
    #> 40                                                     Nature Neuroscience
    #> 41                                                              NeuroImage
    #> 42                                                    Psychological Review
    #> 43                                                                        
    #> 44                                                         Vision Research
    #> 45                                                       Journal of Vision
    #> 46                                                    Psychological Review
    #> 47                                                   Psychological Science
    #> 48                                                    Cognitive Psychology
    #> 49                                                         Vision Research
    #> 50                                                        Visual Cognition
    #> 51                                              Perception & Psychophysics
    #> 52                                                    Cognitive Psychology
    #> 53                                International Journal of Computer Vision
    #> 54                           Progress in Brain Research: Visual Perception
    #> 55                                                                 Science
    #> 56           Journal of Experimental Psychology: Human Learning and Memory
    #> 57                                                                        
    #> 58                                                    Cognitive Psychology
    #> 59                                                       Journal of Vision
    #> 60                                                        Visual Cognition
    #> 61                                                               Cognition
    #> 62                                                   Psychological Science
    #> 63                         Proceedings of the National Academy of Sciences
    #> 64                                                                  Nature
    #> 65                                  Network: Computation in Neural Systems
    #> 66                                                    Cognitive Psychology
    #> 67                                                         Vision Research
    #> 68                                            Evolution and Human Behavior
    #> 69                                          Journal of Economic Psychology
    #> 70                                                                        
    #> 71                                                   Psychological Science
    #> 72                                                           Human Ecology
    #> 73                              Personality and Social Psychology Bulletin
    #> 74                                                                        
    #> 75                                                 Journal of Neuroscience
    #> 76                                                                 Science
    #> 77                                                     Psychiatry Research
    #> 78                                                           Human Ecology
    #> 79                                                                  Nature
    #> 80                                                                        
    #> 81                                                American Economic Review
    #> 82                                                       Economics Letters
    #> 83                                             Philosophy & Public Affairs
    #> 84                                                                 Science
    #> 85                                     Journal of Environmental Psychology
    #> 86                                          American Journal of Psychiatry
    #> 87                                                 Journal of Neuroscience
    #> 88                                              Annual Review of Sociology
    #> 89                                   Journal of Behavioral Decision Making
    #> 90                                                 Neuropsychopharmacology
    #> 91                              Personality and Social Psychology Bulletin
    #> 92                                          Swiss Political Science Review
    #> 93                                                                 Science
    #> 94                                          Journal of Clinical Psychology
    #> 95                                                          Brain Research
    #> 96                                           Trends in Ecology & Evolution
    #> 97                                                                        
    #> 98                                                    Molecular Psychiatry
    #> 99                                                 Journal of Neuroscience
    #> 100                                                               PLoS ONE
    #> 101                                         Journal of Economic Psychology
    #> 102                           Journal of Personality and Social Psychology
    #> 103                                         Journal of Affective Disorders
    #> 104                                                Neuropsychopharmacology
    #> 105                                                               PLoS ONE
    #> 106                        Proceedings of the National Academy of Sciences
    #> 107                                                                       
    #> 108                                                             NeuroImage
    #> 109                                                                       
    #> 110                                               Clinical Neurophysiology
    #> 111                                                                 Neuron
    #> 112                                                                       
    #> 113                                                   Psychological Review
    #> 114                                                Journal of Neuroscience
    #> 115                                                                Science
    #> 116                                                                       
    #> 117                                                    Nature Neuroscience
    #> 118                                          Psychonomic Bulletin & Review
    #> 119                                          Annual Review of Neuroscience
    #> 120                                                                 Neuron
    #> 121                                             Behavioural Brain Research
    #> 122                                                                 Neuron
    #> 123                        Proceedings of the National Academy of Sciences
    #> 124                                                                 Nature
    #> 125                                                                Science
    #> 126                                            Consciousness and Cognition
    #> 127                            Journal of Experimental Psychology: General
    #> 128                      Electroencephalography & Clinical Neurophysiology
    #> 129                                                         Muscle & Nerve
    #> 130                                                         Brain Research
    #> 131                                           Trends in Cognitive Sciences
    #> 132                                       European Journal of Neuroscience
    #> 133                                                                       
    #> 134                                                         Spatial Vision
    #> 135                                                   Psychological Review
    #> 136                                                       Neuropsychologia
    #> 137                   Journal of Clinical and Experimental Neuropsychology
    #> 138                                                                 Neuron
    #> 139                                                 Cognitive Neuroscience
    #> 140                                                                  Brain
    #> 141                                             Journal of Neurophysiology
    #> 142                                                                       
    #> 143                                             Perception & Psychophysics
    #> 144                                                              Cognition
    #> 145 Philosophical Transactions of the Royal Society B: Biological Sciences
    #> 146                                  Frontiers in Integrative Neuroscience
    #> 147                                                 Psychological Bulletin
    #> 148                                                  Psychological Science
    #> 149                                                 Psychological Bulletin
    #> 150                       The Quarterly Journal of Experimental Psychology
    #> 151                        Proceedings of the National Academy of Sciences
    #>     bib_id year_suffix text_id         paper_id
    #> 1        0                 172 0956797613520608
    #> 2        1                 173 0956797613520608
    #> 3        2                 174 0956797613520608
    #> 4        3                 175 0956797613520608
    #> 5        4                 176 0956797613520608
    #> 6        5        <NA>     177 0956797613520608
    #> 7        6                 178 0956797613520608
    #> 8        7                 179 0956797613520608
    #> 9        8                 180 0956797613520608
    #> 10       9                 181 0956797613520608
    #> 11      10                 182 0956797613520608
    #> 12      11                 183 0956797613520608
    #> 13      12                 184 0956797613520608
    #> 14      13                 185 0956797613520608
    #> 15      14                 186 0956797613520608
    #> 16      15                 187 0956797613520608
    #> 17      16                 188 0956797613520608
    #> 18      17                 189 0956797613520608
    #> 19      18                 190 0956797613520608
    #> 20      19                 191 0956797613520608
    #> 21      20                 192 0956797613520608
    #> 22      21                 193 0956797613520608
    #> 23      22                 194 0956797613520608
    #> 24      23                 195 0956797613520608
    #> 25      24                 196 0956797613520608
    #> 26      25                 197 0956797613520608
    #> 27      26                 198 0956797613520608
    #> 28      27                 199 0956797613520608
    #> 29      28                 200 0956797613520608
    #> 30      29                 201 0956797613520608
    #> 31      30                 202 0956797613520608
    #> 32      31                 203 0956797613520608
    #> 33      32                 204 0956797613520608
    #> 34      33                 205 0956797613520608
    #> 35      34                 206 0956797613520608
    #> 36      35                 207 0956797613520608
    #> 37      36                 208 0956797613520608
    #> 38      37                 209 0956797613520608
    #> 39      38                 210 0956797613520608
    #> 40      39                 211 0956797613520608
    #> 41      40                 212 0956797613520608
    #> 42       0                 171 0956797614522816
    #> 43       1                 172 0956797614522816
    #> 44       2                 173 0956797614522816
    #> 45       3                 174 0956797614522816
    #> 46       4                 175 0956797614522816
    #> 47       5           a     176 0956797614522816
    #> 48       6                 177 0956797614522816
    #> 49       7                 178 0956797614522816
    #> 50       8                 179 0956797614522816
    #> 51       9                 180 0956797614522816
    #> 52      10                 181 0956797614522816
    #> 53      11                 182 0956797614522816
    #> 54      12                 183 0956797614522816
    #> 55      13                 184 0956797614522816
    #> 56      14                 185 0956797614522816
    #> 57      15                 186 0956797614522816
    #> 58      16                 187 0956797614522816
    #> 59      17                 188 0956797614522816
    #> 60      18                 189 0956797614522816
    #> 61      19                 190 0956797614522816
    #> 62      20                 191 0956797614522816
    #> 63      21                 192 0956797614522816
    #> 64      22                 193 0956797614522816
    #> 65      23                 194 0956797614522816
    #> 66      24                 195 0956797614522816
    #> 67      25                 196 0956797614522816
    #> 68       0                 190 0956797614527830
    #> 69       1                 191 0956797614527830
    #> 70       2                 192 0956797614527830
    #> 71       3                 193 0956797614527830
    #> 72       4                 194 0956797614527830
    #> 73       5                 195 0956797614527830
    #> 74       6                 196 0956797614527830
    #> 75       7                 197 0956797614527830
    #> 76       8                 198 0956797614527830
    #> 77       9                 199 0956797614527830
    #> 78      10                 200 0956797614527830
    #> 79      11                 201 0956797614527830
    #> 80      12                 202 0956797614527830
    #> 81      13                 203 0956797614527830
    #> 82      14                 204 0956797614527830
    #> 83      15                 205 0956797614527830
    #> 84      16                 206 0956797614527830
    #> 85      17                 207 0956797614527830
    #> 86      18                 208 0956797614527830
    #> 87      19                 209 0956797614527830
    #> 88      20                 210 0956797614527830
    #> 89      21                 211 0956797614527830
    #> 90      22                 212 0956797614527830
    #> 91      23                 213 0956797614527830
    #> 92      24                 214 0956797614527830
    #> 93      25                 215 0956797614527830
    #> 94      26                 216 0956797614527830
    #> 95      27                 217 0956797614527830
    #> 96      28                 218 0956797614527830
    #> 97      29                 219 0956797614527830
    #> 98      30                 220 0956797614527830
    #> 99      31                 221 0956797614527830
    #> 100     32                 222 0956797614527830
    #> 101     33                 223 0956797614527830
    #> 102     34                 224 0956797614527830
    #> 103     35                 225 0956797614527830
    #> 104     36                 226 0956797614527830
    #> 105     37                 227 0956797614527830
    #> 106      0                 236 0956797614557697
    #> 107      1                 237 0956797614557697
    #> 108      2                 238 0956797614557697
    #> 109      3                 239 0956797614557697
    #> 110      4                 240 0956797614557697
    #> 111      5                 241 0956797614557697
    #> 112      6                 242 0956797614557697
    #> 113      7                 243 0956797614557697
    #> 114      8                 244 0956797614557697
    #> 115      9                 245 0956797614557697
    #> 116     10                 246 0956797614557697
    #> 117     11                 247 0956797614557697
    #> 118     12                 248 0956797614557697
    #> 119     13                 249 0956797614557697
    #> 120     14                 250 0956797614557697
    #> 121     15                 251 0956797614557697
    #> 122     16                 252 0956797614557697
    #> 123     17                 253 0956797614557697
    #> 124     18                 254 0956797614557697
    #> 125     19                 255 0956797614557697
    #> 126     20                 256 0956797614557697
    #> 127     21                 257 0956797614557697
    #> 128     22                 258 0956797614557697
    #> 129     23                 259 0956797614557697
    #> 130     24                 260 0956797614557697
    #> 131     25                 261 0956797614557697
    #> 132     26                 262 0956797614557697
    #> 133     27                 263 0956797614557697
    #> 134     28                 264 0956797614557697
    #> 135     29                 265 0956797614557697
    #> 136     30                 266 0956797614557697
    #> 137     31                 267 0956797614557697
    #> 138     32                 268 0956797614557697
    #> 139     33                 269 0956797614557697
    #> 140     34                 270 0956797614557697
    #> 141     35                 271 0956797614557697
    #> 142     36                 272 0956797614557697
    #> 143     37                 273 0956797614557697
    #> 144     38                 274 0956797614557697
    #> 145     39                 275 0956797614557697
    #> 146     40                 276 0956797614557697
    #> 147      0                  53 0956797614560771
    #> 148      1                  54 0956797614560771
    #> 149      2                  55 0956797614560771
    #> 150      3                  56 0956797614560771
    #> 151      4                  57 0956797614560771

## Search Text

You can access a parsed table of the full text of the paper via
`paper$text`, but you may find it more convenient to use the function
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md).
The defaults return a data table of each sentence, with the section
type, header, div, paragraph and sentence numbers, and file name. (The
section type is a best guess from the headers, so may not always be
accurate.)

``` r
text <- search_text(paper)
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 1 | 1 | 1 | Daniel Lakens Lisa DeBruine Jakub Werner | NA | 1 | to_err_is_human | To Err is Human: An Empirical Investigation | unknown |
| 2 | 1 | 2 | 2026-02-22 | NA | 1 | to_err_is_human | To Err is Human: An Empirical Investigation | unknown |
| 3 | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. | NA | 1 | to_err_is_human | Abstract | abstract |
| 4 | 2 | 3 | All data are simulated. | NA | 1 | to_err_is_human | Abstract | abstract |
| 5 | 2 | 3 | The paper shows examples of (1) open and closed OSF links; (2a) citation of retracted papers, (2b) citations without a doi, (2c) citations with Pubpeer comments, (2d) citations in the FLoRA replication database, and (2e) missing/mismatched/incorrect citations and references; (3a) R files with code on GitHub that do not load libraries in one location, (3b) load files that are not shared in the repository, (3c) lack comments, and (3d) have absolute file paths; (4) imprecise reporting of non-significant p-values; (5) tests with and without effect sizes; (6) use of “marginally significant” to describe non-significant findings; (7) a power analysis reporting some of the essential attributes; and (8) retrieving information from preregistrations. | NA | 1 | to_err_is_human | Abstract | abstract |
| 6 | 3 | 4 | Although intentional dishonesty might be a successful way to boost creativity (Gino and Wiltermuth 2014), it is safe to say most mistakes researchers make are unintentional. | NA | 1 | to_err_is_human |  | intro |

### Pattern

You can search for a specific word or phrase by setting the `pattern`
argument. The pattern is a regex string by default; set `fixed = TRUE`
if you want to find exact text matches.

``` r
text <- search_text(paper, pattern = "metacheck")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 3 | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. | NA | 1 | to_err_is_human | Abstract | abstract |
| 9 | 3 | 4 | In this study we examine the usefulness of metacheck to improve best practices. | NA | 1 | to_err_is_human |  | intro |

### Return

Set `return` to one of “sentence”, “paragraph”, “section”, or “match” to
control what gets returned.

``` r
text <- search_text(paper, "GitHub", 
                    return = "paragraph")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|:---|---:|---:|:---|:---|:---|:---|:---|:---|
| NA | 2 | 3 | This paper demonstrates some good and poor practices for use with the {metacheck} R package. All data are simulated. The paper shows examples of (1) open and closed OSF links; (2a) citation of retracted papers, (2b) citations without a doi, (2c) citations with Pubpeer comments, (2d) citations in the FLoRA replication database, and (2e) missing/mismatched/incorrect citations and references; (3a) R files with code on GitHub that do not load libraries in one location, (3b) load files that are not shared in the repository, (3c) lack comments, and (3d) have absolute file paths; (4) imprecise reporting of non-significant p-values; (5) tests with and without effect sizes; (6) use of “marginally significant” to describe non-significant findings; (7) a power analysis reporting some of the essential attributes; and (8) retrieving information from preregistrations. | NA | NA | to_err_is_human | Abstract | abstract |
| NA | 6 | 7 | Data and analysis code is available on GitHub from <https://github.com/Lakens/to_err_is_human> and from <https://researchbox.org/4377>. Data is also available from <https://osf.io/5tbm9> and code is also available from <https://osf.io/629bx>. | NA | NA | to_err_is_human | Data Availability | endnote |

### Regex matches

You can also return just the matched text from a regex search by setting
`return = "match"`. The extra `...` arguments in
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md)
are passed to [`grep()`](https://rdrr.io/r/base/grep.html), so
`perl = TRUE` allows you to use more complex regex, like below.

``` r
pattern <- "[a-zA-Z]\\S*\\s*(=|<)\\s*[0-9\\.,-]*\\d"
text <- search_text(paper, pattern, return = "match", perl = TRUE)
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|
| 19 | 7 | 8 | N=50 | NA | 2 | to_err_is_human | Power Analysis | method |
| 21 | 8 | 10 | M=9.12 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | M=10.9 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | t(97.7)=2.9 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | p=0.005 | NA | 3 | to_err_is_human | Results | results |
| 21 | 8 | 10 | d=0.59 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | M=5.06 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | M=4.5 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | t(97.2)=-1.96 | NA | 3 | to_err_is_human | Results | results |
| 22 | 8 | 11 | p=0.152 | NA | 3 | to_err_is_human | Results | results |
| 39 | 16 | 25 | pwr::pwr.t.test(n = 50 | NA | 2 | to_err_is_human | Footnote 2 | footnote |
| 39 | 16 | 25 | power = 0.8 | NA | 2 | to_err_is_human | Footnote 2 | footnote |

### Expand Text

You can expand the text returned by
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md)
or a module with
[`expand_text()`](https://scienceverse.github.io/metacheck/dev/reference/expand_text.md).

``` r
marginal <- search_text(paper, "marginal") |>
  expand_text(paper, plus = 1, minus = 1)

marginal[, c("text", "expanded")]
```

    #> # A tibble: 2 × 2
    #>   text                                                                  expanded
    #>   <chr>                                                                 <chr>   
    #> 1 "The paper shows examples of (1) open and closed OSF links; (2a) cit… "All da…
    #> 2 "On average researchers in the experimental condition found the app … "On ave…

## Large Language Models

You can query the extracted text of papers with LLMs using any models
supported by [ellmer](https://ellmer.tidyverse.org/).

### Setup

You will need to get **your own API key** (the one below is a fake
example) from your preferred provider
(e.g. <https://console.groq.com/keys>). To avoid having to type it out,
add it to the .Renviron file in the following format (you can use
[`usethis::edit_r_environ()`](https://usethis.r-lib.org/reference/edit.html)
to access the .Renviron file).

``` bash
GROQ_GPT_KEY="sk-proj-abcdefghijklmnopqrs0123456789ABCDEFGHIJKLMNOPQRS"
```

``` r
# useful if you aren't sure where this file is
usethis::edit_r_environ()
```

You can get or set the default LLM model with
[`llm_model()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model.md)
and access a list of the current available models using
[`llm_model_list()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model_list.md).

| platform | id | object | owned_by | context_window | max_completion_tokens | created_at |
|:---|:---|:---|:---|---:|---:|:---|
| groq | canopylabs/orpheus-arabic-saudi | model | Canopy Labs | 4000 | 50000 | 2025-12-16 |
| groq | meta-llama/llama-4-scout-17b-16e-instruct | model | Meta | 131072 | 8192 | 2025-04-05 |
| groq | openai/gpt-oss-safeguard-20b | model | OpenAI | 131072 | 65536 | 2025-10-29 |
| groq | meta-llama/llama-prompt-guard-2-22m | model | Meta | 512 | 512 | 2025-05-30 |
| groq | groq/compound-mini | model | Groq | 131072 | 8192 | 2025-09-04 |

When you start metacheck for the first time, it will check for relevant
API keys in your Renviron and automatically set the model to use. You
can get or set this with
[`llm_model()`](https://scienceverse.github.io/metacheck/dev/reference/llm_model.md).

``` r
llm_model() # get current model
llm_model("groq") # set to ellmer's default groq model
llm_model("groq/llama-3.3-70b-versatile") # set to specific openai model
```

### LLM Queries

You can query the extracted text of papers with LLMs. See
[`?llm`](https://scienceverse.github.io/metacheck/dev/reference/llm.md)
for details of how to get and set up your API key, choose an LLM, and
adjust settings.

Use
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md)
first to narrow down the text into what you want to query. Below, we
limited search to the first ten papers, and returned sentences that
contains the word “power” and at least one number. Then we asked an LLM
to determine if this is an a priori power analysis, and if so, to return
some relevant values in a JSON-structured format.

``` r
power <- psychsci[1:10] |>
  # sentences containing the word power
  search_text("power") |>
  # and containing at least one number
  search_text("[0-9]") 

# ask a specific question with specific response format
system_prompt <- 'Does this sentence report an a priori power analysis? If so, return the test, sample size, critical alpha criterion, power level, effect size and effect size metric plus any other relevant parameters, in JSON format like:

{
  "apriori": true, 
  "test": "paired samples t-test", 
  "sample": 20, 
  "alpha": 0.05, 
  "power": 0.8, 
  "es": 0.4, 
  "es_metric": "cohen\'s D"
}

If not, return {"apriori": false}

Answer only in valid JSON format, starting with { and ending with }.'

llm_power <- llm(power, system_prompt)
```

### Expand JSON

It is useful to ask an LLM to return data in JSON structured format, but
can be frustrating to extract the data, especially where the LLM makes
syntax mistakes. The function
[`json_expand()`](https://scienceverse.github.io/metacheck/dev/reference/json_expand.md)
tries to expand a column with a JSON-formatted response into columns and
deals with it gracefully (sets an ‘error’ column to “parsing error”) if
there are errors. It also fixes column data types, if possible.

``` r
llm_response <- json_expand(llm_power, "answer") |>
  dplyr::select(text, apriori:es_metric)
```

| text | apriori | test | sample | alpha | power | es | es_metric |
|:---|:---|:---|---:|---:|---:|---:|:---|
| It is possible that less-consistent effects were observed on trials with errors because of reduced power to detect an effect on these trials, which by design were less numerous (~25%). | FALSE | NA | NA | NA | NA | NA | NA |
| Figure 1 shows that CY had very little predictive power for CLIM, but the fit in the transposed plot has an obvious bell-shaped curve. | FALSE | NA | NA | NA | NA | NA | NA |
| Sample size was calculated with an a priori power analysis, using the effect sizes reported by Küpper et al. (2014), who used identical procedures, materials, and dependent measures. | TRUE | NA | NA | NA | NA | NA | NA |
| We determined that a minimum sample size of 7 per group would be necessary for 95% power to detect an effect. | TRUE | t-test | 7 | 0.050 | 0.95 | NA | NA |
| For the first part of the task, 11 static visual images, one from each of the scenes in the film were presented once each on a black background for 2 s using Power-Point. | FALSE | NA | NA | NA | NA | NA | NA |
| A sample size of 26 per group was required to ensure 80% power to detect this difference at the 5% significance level. | TRUE | two-sample t-test | 26 | 0.050 | 0.80 | NA | NA |
| A sample size of 18 per condition was required in order to ensure an 80% power to detect this difference at the 5% significance level. | TRUE | t-test | 18 | 0.050 | 0.80 | NA | NA |
| The 13,500 selected loan requests conservatively achieved a power of .98 for an effect size of .07 at an alpha level of .05. | TRUE |  | 13500 | 0.050 | 0.98 | 0.07 | NA |
| On the basis of simulations over a range of expected effect sizes for contrasts of fMRI activity, we estimated that a sample size of 24 would provide .80 power at a conservative brainwide alpha threshold of .002 (although such thresholds ideally should be relaxed for detecting activity in regions where an effect is predicted). | TRUE | fMRI activity contrast | 24 | 0.002 | 0.80 | NA | NA |
| Stimulus sample size was determined via power analysis of the sole existing similar study, which used neural activity to predict Internet downloads of music (Berns & Moore, 2012). | TRUE | NA | NA | NA | NA | NA | NA |
| The effect size from that study implied that a sample size of 72 loan requests would be required to achieve .80 power at an alpha level of .05. | TRUE |  | 72 | 0.050 | 0.80 | NA | NA |
| Categorical ratings of the emotional expressions in the loan photographs had a similarly powerful impact on loan-request success; requests with “happy” photographs received \$5.15 more per hour than requests with “sad” photographs, on average; they achieved full funding in 7.6% less time. | FALSE | NA | NA | NA | NA | NA | NA |
| Although previous research has provided mixed evidence about the impact of positive versus negative affect on charitable giving (Andreoni, 1990;Small & Verrochi, 2009), by simultaneously assessing affect at both Internet-aggregate and laboratory-sample levels of analysis, our studies provide consistent evidence that photograph-elicited positive arousal most powerfully promoted lending rates and outcomes (Tables 1 and 2, Fig. 2a, and Fig. | FALSE | NA | NA | NA | NA | NA | NA |

### Rate Limiting

The
[`llm()`](https://scienceverse.github.io/metacheck/dev/reference/llm.md)
function makes a separate query [^1] for each row in a data frame from
[`search_text()`](https://scienceverse.github.io/metacheck/dev/reference/search_text.md).
To prevent accidentally making way too many calls because of errors in
your code, we set the default limits to 30 queries at a time, but you
can change this:

``` r
llm_max_calls(30)
```

## OSF Functions

Metacheck provides several function to help you assess resources
archived on the Open Science Framework.

### OSF Links and IDs

Get any OSF links from a paper or list of papers.

``` r
links <- osf_links(psychsci)

links$text |> unique() |> head()
```

    #> [1] "osf.io/e2aks"                  "osf.io/tvyxz/"                
    #> [3] "osf.io/t9j8e/? view_only=f171" "osf .io/ideta"                
    #> [5] "osf.io/tvyxz/ "                "osf.io/eky4s"

You can see that some of them have rogue spaces or view-only links. The
function
[`osf_check_id()`](https://scienceverse.github.io/metacheck/dev/reference/osf_check_id.md)
takes most formats of OSF links (with or without <https://> and osf.io/,
as well as the 25-character waterbutler IDs) and converts them to short
IDs.

``` r
osf_ids <- osf_check_id(links$text) |> unique()

head(osf_ids)
```

    #> [1] "e2aks" "tvyxz" "t9j8e" "ideta" "eky4s" "xgwhk"

However, all of the `osf_***()` functions fix IDs for you and handle
duplicate IDs without making extra API calls, so you don’t need to add
this step to most workflows.

### OSF Info

Get basic information about OSF links, such as the name, description,
osf_type (nodes, files, preprints, registrations, users, set to
“private” if you don’t have authorisation to view it, and “invalid” if
the ), whether it is public

``` r
info <- osf_retrieve(links[1:6, "text"])

info[, c("text","osf_id", "osf_type", "public", "category")]
```

    #> # A tibble: 6 × 5
    #>   text                            osf_id osf_type public category
    #>   <chr>                           <chr>  <chr>    <lgl>  <chr>   
    #> 1 "osf.io/e2aks"                  e2aks  nodes    TRUE   project 
    #> 2 "osf.io/tvyxz/"                 tvyxz  nodes    TRUE   project 
    #> 3 "osf.io/tvyxz/"                 tvyxz  nodes    TRUE   project 
    #> 4 "osf.io/t9j8e/? view_only=f171" t9j8e  private  FALSE  NA      
    #> 5 "osf .io/ideta"                 ideta  nodes    TRUE   project 
    #> 6 "osf.io/tvyxz/ "                tvyxz  nodes    TRUE   project

For now, the OSF API does not let us retrieve any information about
view-only links. They may be viewable by you in the web browser if the
link is still active, but will be listed in the table as public = FALSE
and osf_type = “private”.

You can set the argument `recursive = TRUE` to also retrieve information
about all nodes and files that are contained by the OSF link.

``` r
osf_api_calls(0)
all_contents <- osf_retrieve(links$text[1], recursive = TRUE)
n_calls <- osf_api_calls()
```

The function
[`osf_api_calls()`](https://scienceverse.github.io/metacheck/dev/reference/osf_api_calls.md)
lets you reset and retrieve the number of API calls made since the last
reset. You can see that the project osf.io/e2aks had 3 nodes and 6
files, which required 10 API calls.

``` r
sum(all_contents$osf_type == "nodes")
```

    #> [1] 3

### Download OSF Files

OSF projects let you organise information into nested components, and
files within those components. Therefore, to retrieve all of the files
associate with a project, you may need to navigate to several components
and download zip files for the files from each components, then
reorganise and rename the downloaded folders.

The function
[`osf_file_download()`](https://scienceverse.github.io/metacheck/dev/reference/osf_file_download.md)
does all of this for you, recreating a folder structure based on the
component names and downloading all files smaller than `max_file_size`
(defaults to 10 MB) up to a total size of `max_download_size` (defaults
to 100 MB).

``` r
osf_file_download(osf_id = "pngda",
                  download_to = ".", 
                  max_file_size = 1, 
                  max_download_size = 10)
```

    Starting retrieval for pngda
    - omitting metacheck.png (1.5MB)
    Downloading files [=====================] 24/24 00:00:35

``` r
list.files("pngda", recursive = TRUE)
```

    #>  [1] "Data/Individual/data-01.csv"                         
    #>  [2] "Data/Individual/data-02.csv"                         
    #>  [3] "Data/Individual/data-03.csv"                         
    #>  [4] "Data/Individual/data-04.csv"                         
    #>  [5] "Data/Individual/data-05.csv"                         
    #>  [6] "Data/Individual/data-06.csv"                         
    #>  [7] "Data/Individual/data-07.csv"                         
    #>  [8] "Data/Individual/data-08.csv"                         
    #>  [9] "Data/Individual/data-09.csv"                         
    #> [10] "Data/Individual/data-10.csv"                         
    #> [11] "Data/Individual/data-11.csv"                         
    #> [12] "Data/Individual/data-12.csv"                         
    #> [13] "Data/Individual/data-13.csv"                         
    #> [14] "Data/Individual/data-14.csv"                         
    #> [15] "Data/Processed Data/processed-data.csv"              
    #> [16] "Data/Raw Data/data.xlsx"                             
    #> [17] "Data/Raw Data/nest-1/nest-2/nest-3/nest-4/test-4.txt"
    #> [18] "Data/Raw Data/nest-1/nest-2/nest-3/test-3.txt"       
    #> [19] "Data/Raw Data/nest-1/nest-2/test-2.txt"              
    #> [20] "Data/Raw Data/nest-1/README"                         
    #> [21] "Data/Raw Data/nest-1/test-1.txt"                     
    #> [22] "Data/Raw Data/README"                                
    #> [23] "README"

## Modules

metacheck is designed modularly, so you can add modules to check for
anything. It comes with a set of pre-defined modules, and we hope people
will share more modules.

### Module List

You can see the list of built-in modules with the function below.

``` r
module_list()
```

    #> 
    #> *** GENERAL ***
    #> 
    #> * all_urls: List all the URLs in the main text.
    #> * coi_check: Identify and extract Conflicts of Interest (COI) statements.
    #> * coi_check_oi: Identify and extract Conflicts of Interest (COI) statements.
    #> * funding_check: Identify and extract funding statements.
    #> * funding_check_oi: Identify and extract funding statements.
    #> * open_practices: This module incorporates ODDPub into metacheck. ODDPub is a text mining algorithm that detects which publications disseminated Open Data or Open Code together with the publication.
    #> 
    #> *** METHOD ***
    #> 
    #> * causal_claims: Aims to identify the presence of random assignment, and lists sentences that make causal claims in title or abstract.
    #> * power: This module uses uses regular expressions to identify sentences that contain a statistical power analysis. If specified by the user, it also uses a large language module (LLM) to extract information reported in power analyses, including the statistical test, sample size, alpha level, desired level of power, and magnitude and type of effect size.
    #> * prereg_check: Retrieve information from preregistrations in a standardised way,
    #> and make them easier to check.
    #> 
    #> *** RESULTS ***
    #> 
    #> * all_p_values: List all p-values in the text, returning the matched text (e.g., 'p = 0.04') and document location in a table.
    #> * code_check: This module retrieves information from repositories checked by repo_check about code files (R, SAS, SPSS, Stata).
    #> * marginal: List all sentences that describe an effect as 'marginally significant'.
    #> * repo_check: This module retrieves information from repositories.
    #> * stat_check: Check consistency of p-values and test statistics
    #> * stat_effect_size: The Effect Size module checks for effect sizes in t-tests and F-tests.
    #> * stat_p_exact: List any p-values reported with insufficient precision (e.g., p < .05 or p = n.s.)
    #> * stat_p_nonsig: This module checks for imprecisely reported p values. If p > .05 is detected, it warns for misinterpretations.
    #> 
    #> *** REFERENCE ***
    #> 
    #> * ref_accuracy: This module checks references for mismatches with CrossRef.
    #> * ref_consistency: Check if all references are cited and all citations are referenced
    #> * ref_miscitation: Check for frequently miscited papers. This module is just a proof of concept -- the miscite database is not yet populated with real examples.
    #> * ref_pubpeer: This module checks references and warns for citations that have comments on pubpeer (excluding Statcheck comments).
    #> * ref_replication: This module checks references and warns for citations of original studies for which replication or reproduction studies exist in the FLoRA database.
    #> * ref_retraction: This module checks references and warns for citations in the RetractionWatch Database.
    #> * ref_summary: Summarise information about each reference in a paper.
    #> 
    #> Use `module_help("module_name")` for help with a specific module

### Running modules

To run a built-in module on a paper, you can reference it by name.

``` r
p <- module_run(paper, "all_p_values")
```

| text_id | section_id | paragraph_id | text | formatted | page_number | paper_id | header | section_type | p_comp | p_value |
|---:|---:|---:|:---|:---|---:|:---|:---|:---|:---|---:|
| 21 | 8 | 10 | p=0.005 | NA | 3 | to_err_is_human | Results | results | = | 0.005 |
| 22 | 8 | 11 | p=0.152 | NA | 3 | to_err_is_human | Results | results | = | 0.152 |
| 23 | 8 | 12 | p \> .05 | NA | 3 | to_err_is_human | Results | results | \> | 0.050 |

### Creating modules

You can create your own modules using R code. Modules can also contain
instructions for reporting, to give “traffic lights” for whether a check
passed or failed, and to include appropriate text feedback in a report.
See the [modules
vignette](https://scienceverse.github.io/metacheck/dev/articles/modules.md)
for more details.

## Reports

You can generate a report from any set of modules. Check the function
help for the default set.

``` r
report(paper, output_format = "qmd")
```

See the [example
report](https://scienceverse.github.io/metacheck/dev/report-example.md).

[^1]: Using the parallel functions in ellmer can be more efficient, but
    currently doesn’t do a good job of associating structured output to
    the input text when input may have 0+ outputs.
