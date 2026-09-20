---
title: Index
type: overview
updated: 2026-09-21
---

# Wiki Index

Catalogue of every page in this wiki. See `CLAUDE.md` for conventions.

**Sources ingested:** L01–L13 — **all 13 lectures** — `BioinspiredAIWdh1und2.pdf`, `BioinspiredAIWdh3.pdf`, `BioinspiredAIWdh4.pdf`, `BioinspiredAIWdh5.pdf`, `BioinspiredAIWdh6.pdf`, `BioinspiredAIWdh7.pdf`, `BioinspiredAIWdh8.pdf`, `BioinspiredAIWdh9.pdf`, `BioinspiredAIWdh10.pdf`, `BioinspiredAIWdh11.pdf`, `BioinspiredAIWdh12.pdf`, `BioinspiredAIWdh13.pdf`

## Lectures

- [[L01-introduction-to-bio-inspired-ai]] — framing, the two requirements for being bio-inspired, and the navigation example. `L01` · solid
- [[L02-spiking-neural-networks]] — from the action potential to spiking models and STDP; the module's first technical lecture. `L02` · solid
- [[L03-computational-neural-networks]] — abstraction down to nodes; perceptron learning, backprop, training issues, RNNs, learning paradigms. `L03` · solid
- [[L04-embodied-language-processing]] — language in the brain, the embodied thesis, SOMs, grounding by imitation, Word2Vec/GPT/HuBERT. `L04` · solid
- [[L05-robot-sound-localisation]] — ITD/ILD, the Jeffress model, cross-correlation, and the module's first complete working system. `L05` · solid
- [[L06-hierarchical-vision]] — retina to V1 to CNN; the module's best-evidenced correspondence, and its sharpest plausibility critique. `L06` · solid
- [[L07-crossmodal-processing]] — why senses are combined, the superior colliculus, optimal cue integration, and fusion architectures. `L07` · solid
- [[L08-behaviour-based-robotics]] — against functional decomposition; Braitenberg, subsumption, motor schemas, and NICO. `L08` · solid
- [[L09-bio-inspired-attention]] — attention defined at last: the four kinds, three networks, saliency, and the cocktail party. `L09` · solid
- [[L10-gesture-recognition]] — gestures as a continuum; MCCNN, CNN-LSTM, the snapshot model, and a network that grows its own architecture. `L10` · solid
- [[L11-evolutionary-computing]] — the module's other tradition: four pillars, the EA scheme, every operator, and evolving networks. `L11` · solid
- [[L12-neuro-symbolic-and-explainable-ai]] — the two traditions compared, four ways to hybridise them, and every method the module offers for reading a network out. `L12` · solid
- [[L13-continual-learning]] — the module's best lecture: catastrophic forgetting, four strategies, the full GWR algorithm, and its only evaluation metrics. `L13` · solid

## Concepts

- [[acoustic-shadow]] — the head blocks short wavelengths; the basis of duplex theory. `L05` · solid
- [[action-potential]] — all-or-none membrane spike; −70 rest, −55 threshold, +40 peak, 3–50 ms. `L02` · solid
- [[activation-function]] — identity, step, sigmoid, ReLU, tanh; the five sigmoid properties. `L02, L03, L10` · solid
- [[additive-factors-method]] — infer whether two variables hit the same processing stage; and why the inference runs one way only. `L09` · solid
- [[ann-brain-correspondence]] — L03's mapping table, and why "backpropagation ↔ plasticity" is weak — and L05's ITD correspondence, which holds. `L03, L04, L05, L06, L07, L09, L11, L12, L13` · solid
- [[attention]] — selection under limited capacity — and why it is not what transformers mean by the word. `L09` · solid
- [[attention-networks]] — alerting, orienting, executive control; the three functional networks and their areas. `L09` · solid
- [[auditory-pathway]] — ear to auditory nerve to MSO/LSO/IC; where ITD and ILD are computed. `L05, L07` · solid
- [[auditory-scene-analysis]] — organising a mixed waveform into sources; the cocktail party answer. `L09` · solid
- [[azimuth-and-elevation]] — sound's two coordinates; only azimuth is ever solved. `L05` · solid
- [[batch-vs-online-training]] — online, batch, mini-batch; learning rate and momentum. `L03` · solid
- [[behaviour-coordination]] — the function C: competitive (subsumption, selection, voting) vs cooperative (vector sum). `L08` · solid
- [[candidate-representation]] — cover all solutions, allow only valid ones — and why bit-strings break locality. `L11` · solid
- [[catastrophic-forgetting]] — new learning overwrites old; why distributed representation causes it. `L13` · solid
- [[cocktail-party-problem]] — perception in auditory clutter; stated as motivation, not solved. `L05` · stub
- [[competing-conventions-problem]] — n! genomes per function; why recombining two good networks usually breaks both. `L11` · solid
- [[complementary-learning-systems]] — fast hippocampal and slow cortical memory; named but never defined. `L13` · stub
- [[compositionality-of-language]] — humans understand unseen compositions; the model makes similar mistakes. `L04` · solid
- [[continual-language-learning]] — CL for NLP; why pretrain-then-fine-tune is structurally not continual. `L13` · solid
- [[continual-learning]] — learn over time, no re-access to old data, bounded resources. `L13` · solid
- [[continual-learning-metrics]] — average accuracy and forgetting — the module's only evaluation measures. `L13` · solid
- [[continual-learning-strategies]] — regularisation, dynamic architectures, replay, hybrid — and what each costs. `L13` · solid
- [[cross-modal-stimuli-prediction]] — one SOM shared between visual and auditory encode/decode paths. `L04, L07` · developing
- [[data-augmentation]] — multiple translations shown simultaneously; a soft substitute for weight sharing. `L06, L08, L10` · developing
- [[deep-network-tradeoffs]] — the module's only cost/benefit audit of deep learning — and its only negative result. `L10, L11` · solid
- [[developmental-and-curriculum-learning]] — critical periods and curricula of increasing complexity. `L13` · solid
- [[dna-and-heredity]] — nucleotides, mitosis, meiosis, crossing-over — the biological originals of the operators. `L11` · solid
- [[dropout]] — zero ≈50% of activations during training. `L03` · developing
- [[dual-stream-hypothesis]] — language as pathways, not boxes; the dorsal/ventral labels are swapped — **confirmed at L06**. `L04, L06` · developing
- [[dynamic-weight-sharing]] — lateral connections plus a sleep-phase local rule; the module's best plausibility result. `L06` · solid
- [[embodied-language-representation]] — language is embodied, distributed, whole-brain; word-webs. `L04, L10` · solid
- [[embodiment-and-situatedness]] — Brooks's two core ideas, and how they differ from L04's sense of embodiment. `L08, L11` · solid
- [[excitatory-and-inhibitory-neurons]] — the two neuron types, carried as weight sign. `L02, L08` · developing
- [[exogenous-and-endogenous-attention]] — bottom-up vs top-down — the same fork as reactive vs deliberative. `L09` · solid
- [[explainable-ai]] — what counts as an explanation, and the three incompatible senses L12 uses. `L12` · solid
- [[feature-integration-theory]] — preattentive features, attentive binding; the location map as join key. `L09` · solid
- [[fitness-function]] — the only place the problem enters; watch best and average fitness together. `L11` · solid
- [[fitness-landscape]] — local and global optima, uni- and multi-modal — defined by the operators, not the problem. `L11` · solid
- [[four-pillars-of-evolution]] — population, diversity, heredity, selection — substrate-neutral, and identical to the algorithm. `L11` · solid
- [[functional-decomposition]] — perception→modelling→planning→execution, and why the serial pipeline fails on robots. `L08` · solid
- [[fusion-strategies]] — early, intermediate and late fusion, plus four simple join types; where two streams meet. `L07, L08` · solid
- [[genetic-neural-encoding]] — weights, transfer parameters, topology — and why genome layout decides what crossover preserves. `L11` · solid
- [[genotype-and-phenotype]] — variation acts on one, selection on the other; direct and generative encodings. `L11` · solid
- [[geometric-sound-localisation]] — a = c·t_ITD, Θ = arccos(a/b), with a worked example. `L05` · solid
- [[gesture-continuum]] — gesticulation → sign language, ordered by conventionalisation and independence from speech. `L10` · solid
- [[gesture-phases]] — rest → pre-stroke → stroke → post-stroke → rest; only the stroke carries meaning. `L10` · solid
- [[gesture-representation]] — appearance-based vs model-based — and how skeletal input dissolves the fork. `L10` · solid
- [[grid-cells]] — form a coordinate system for navigation. `L01` · stub
- [[hebbian-learning]] — "cells that fire together, wire together"; its three problems, and how the module widens "Hebbian" to mean "local". `L02, L04, L06, L13` · solid
- [[human-pose-estimation]] — 2D/3D, single/multi-person, regression vs part detection, top-down vs bottom-up. `L10` · solid
- [[hybrid-architecture]] — algorithm where the physics is known, learning where it isn't. `L05, L07, L08, L09, L10, L12` · solid
- [[hybrid-integration-architectures]] — loose coupling, tight coupling, full integration — a general vocabulary for combining anything. `L12` · solid
- [[imitation-learning]] — copying a demonstrated task — and why the pipeline is the one L08 rejects. `L08` · solid
- [[intelligent-behaviour]] — the five capabilities and the two requirements defining bio-inspired AI, tested against L05. `L01, L05, L08` · solid
- [[interaural-level-difference]] — level difference in dB; computed in the LSO; best at high frequencies. `L05` · solid
- [[interaural-time-difference]] — arrival-time difference; computed in the MSO; the cue that requires spike timing. `L05` · solid
- [[intrinsic-motivation]] — curiosity, internal reward, self-generated curricula. `L13` · solid
- [[inverse-effectiveness]] — the weaker the unisensory responses, the larger the multisensory gain. `L07` · solid
- [[inverse-kinematics]] — pose → joint configuration; many solutions, no solution, or no closed form. `L11` · solid
- [[knowledge-extraction]] — turning a trained network's weights and activations back into readable rules. `L12` · solid
- [[language-areas-of-the-brain]] — Broca, Wernicke, the two aphasias, and why the model was rejected. `L04` · solid
- [[learning-paradigms]] — supervised, unsupervised, reinforced, and (from L04) self-supervised. `L03, L04, L08, L09, L10, L11, L12, L13` · solid
- [[levels-of-abstraction]] — structural, functional, temporal; and the reset policy that selects the model. `L03` · solid
- [[levels-of-abstraction]] — edges → parts → objects, reused as an argument that deep nets are legible. `L12` · solid
- [[linear-separability]] — a neuron cuts the input space with a hyperplane. `L02, L03` · solid
- [[local-minima-problem]] — stalls and cycles in vector-summed navigation; why noise and avoid-past are patches. `L08, L11` · solid
- [[local-vs-distributed-representation]] — grandmother cells vs feature codes; and codes defined by what they discard. `L02, L04, L06, L07, L09, L12, L13` · solid
- [[loss-function]] — mean squared error and its regularised form. `L03` · solid
- [[memory-replay]] — cortico-hippocampal consolidation; the module's only falsifiable result. `L13` · solid
- [[modality-appropriateness-hypothesis]] — each modality dominates the dimension it is best suited to; vision wins space. `L07` · solid
- [[motion-history-image]] — collapse a sequence into one image whose intensity encodes recency of motion. `L10` · solid
- [[motion-intensity-profile]] — ISSIM against the first frame; a scalar curve whose peaks are the strokes. `L10` · solid
- [[motor-schema]] — independently defined behaviours combined by weighted vector sum. `L08` · solid
- [[multisensory-integration]] — combining modalities for robustness, disambiguation and completeness. `L07, L13` · solid
- [[mutation]] — one parent, small step — and the neutral structural mutations that make topology search work. `L11` · solid
- [[network-architectures]] — feed-forward, recurrent, competitive, attention, hybrid pipelines, convolutional vs locally connected. `L02, L03, L04, L05, L06, L07` · developing
- [[neural-coding]] — the fork: activity level vs frequency/timing; connectionism vs computational neuroscience. `L02` · solid
- [[neural-similarity-and-dot-product]] — a neuron's output measures input–weight alignment; L05's correlator and L06's convolution are the same measure. `L02, L05, L06` · solid
- [[neural-symbolic-integration]] — why and how to join the two paradigms; the comparison table and the integration taxonomy. `L12` · solid
- [[optimal-cue-integration]] — reliability-weighted averaging; the inverse-variance rule and its Bayesian reading. `L07` · solid
- [[orientation-tuning]] — V1 cells prefer an edge angle, with graded falloff; the fifth ordered-population code. `L06, L09` · solid
- [[overfitting-and-underfitting]] — adapting to noise vs failing to adapt to structure. `L03` · solid
- [[parent-selection]] — probabilistic by design; a steady removal of global knowledge across four methods. `L11` · solid
- [[perceptron-convergence-theorem]] — separable data ⇒ a hyperplane in finite steps. `L03` · solid
- [[perceptron-learning-rule]] — Δw = η(t−y)x; the first supervised rule. `L03, L10` · solid
- [[phrenology]] — skull bumps as brain areas; the wrong method that got a half-right answer. `L04` · solid
- [[place-cells]] — inner map of the environment; one cell per place. `L01, L07` · developing
- [[pooling]] — max or average over a window; the complex cell, and how invariance is bought. `L06` · solid
- [[pop-out-effect]] — saliency is a local difference, not a property of the object. `L09` · solid
- [[potential-field-navigation]] — attractive and repulsive fields; movement predetermined by descent. `L08` · solid
- [[premature-convergence]] — diversity is the resource selection consumes; five causes, five scattered remedies. `L11` · solid
- [[rate-coding]] — activity as a single number; the basis of connectionism. `L02` · solid
- [[reaction-time]] — the module's first behavioural measure; difference scores and their logic. `L09` · solid
- [[reactive-agent]] — action as an immediate function of sensors; the full balance sheet. `L08` · solid
- [[receptive-field]] — the region that alters a neuron's firing; ON/OFF centre-surround. Also the CNN kernel. `L06` · solid
- [[recombination]] — two parents, one jump — the only non-local move in the module, and the only destructive one. `L11` · solid
- [[refractory-period]] — the recovery window, and the two ways models implement it. `L02` · solid
- [[regularisation]] — penalise weights; note the formula in the source is malformed. `L03` · developing
- [[reservoir-computing]] — named once, in a summary, for action selection in prefrontal cortex and striatum. `L10` · solid
- [[saliency-map]] — one feature-blind priority map; the claim that V1 already carries it. `L09` · solid
- [[selection-pressure]] — one dial from drift to greed; the module's fifth architectural axis. `L11` · solid
- [[self-supervised-learning]] — the fourth paradigm: targets built out of the input. `L04` · solid
- [[simple-complex-hypercomplex-cells]] — V1's ladder: selectivity traded for invariance, twice. `L06` · solid
- [[social-attention]] — gaze-following, and measured effects of robot gaze on trust and rated intelligence. `L09` · solid
- [[spatial-principle]] — co-located cross-modal stimuli enhance, disparate ones depress. `L07` · solid
- [[stability-plasticity-dilemma]] — retain versus adapt — the module's sixth architectural axis. `L13` · solid
- [[static-and-dynamic-gestures]] — posture vs trajectory, and the segmentation problem only the second one has. `L10` · solid
- [[stdp]] — Hebbian learning with temporal asymmetry; weights can now decrease. `L02` · solid
- [[superior-colliculus]] — topographically organised midbrain map where the senses meet and orienting is decided. `L07` · solid
- [[survivor-selection]] — μ + λ → μ, deterministically; elitism versus turnover. `L11` · solid
- [[symbol-grounding]] — how a word gets attached to a thing; basic vs higher-order grounding. `L04` · solid
- [[symbolic-ai]] — rules, logic and explicit knowledge — the tradition the module spent twelve lectures not being. `L12` · solid
- [[synaptic-kernel]] — per-synapse temporal characteristics; input convolved with u_ij. `L02` · developing
- [[synaptic-plasticity]] — synapses strengthening or weakening over time; the schema all learning rules fill. `L02, L13` · solid
- [[temporal-coding]] — three spike encoding schemes; vindicated by ITD in L05. `L02, L05` · solid
- [[the-retina]] — layer stack, rods and cones; light enters from the back. `L06, L09` · solid
- [[tonotopic-representation]] — frequency mapped onto position; the ear's place code. `L05` · solid
- [[top-down-modulation]] — cortex biases rather than drives subcortical integration. `L07, L08, L09, L12` · solid
- [[transfer-learning]] — primitives reused for compositions; pretrain-then-adapt. `L04, L13` · solid
- [[two-visual-streams]] — ventral *what* / dorsal *where*; the finding that proves L04's labels were swapped. `L06, L04` · solid
- [[uncanny-valley]] — named as the thing NICO's child-like face avoids; never defined. `L08` · stub
- [[unity-assumption]] — whether the brain treats two signals as one event; the precondition for fusing them. `L07` · developing
- [[vanishing-gradient-problem]] — gradient becomes small → no update. `L03` · solid
- [[ventriloquism-effect]] — vision captures the perceived location of sound; two competing explanations, never adjudicated. `L07, L09` · solid
- [[visual-pathway]] — retina to LGN and three other subcortical targets; V1–V5. `L06, L07` · solid
- [[weight-sharing]] — what makes a CNN work, and the one thing real neurons cannot do. `L06` · solid
- [[winner-take-all]] — argmax by lateral inhibition; the module's fifth encounter with one primitive. `L09, L11` · solid
- [[word-embedding]] — dense learned vectors replace atomic one-hot words. `L04` · solid
- [[xor-problem]] — OR and AND are learnable, XOR is not. `L03` · solid

## Systems

- [[associative-gwr]] — classification by label histogram on each neuron. `L13` · solid
- [[attention-network-test]] — three RT difference scores from one task — and why the third has opposite polarity. `L09` · solid
- [[auditory-attention-model]] — group, segregate, compete, with top-down bias at every stage. `L09` · solid
- [[autoencoder]] — error based on reconstruction quality. `L03` · stub
- [[automata-extraction]] — clustering an RNN's state space into a finite state machine. `L12` · solid
- [[backpropagation]] — error derivatives propagated backwards; the three steps and the δ recursion. `L03, L08, L11` · solid
- [[bert]] — transformer LM; unsupervised pretraining then supervised fine-tuning. `L13` · stub
- [[braitenberg-vehicle]] — four wires, four temperaments; the module's hardest case against inferring mechanism from behaviour. `L08, L11` · solid
- [[carl]] — named as the rehearsal example; acronym never expanded. `L13` · stub
- [[class-activation-map]] — which region of an image supported a specific class. `L12` · solid
- [[cnn-lstm]] — CNN for invariance in space, LSTM for invariance in time; frame-level vs sequence-level. `L10` · solid
- [[collision-free-navigation]] — Φ = V(1−√ΔV)(1−i) — a product, not a sum, and navigation emerges from it. `L11` · solid
- [[continuous-dynamic-neuron]] — the ODE form; μ ↔ 1/τ; an RC circuit. `L02` · solid
- [[contrastive-language-image-pretraining]] — CLIP — align image and text encoders contrastively; zero-shot by typing a class name. `L10` · solid
- [[convolutional-network]] — LeNet-5, convolution arithmetic, pooling; derived from the visual cortex, broken by weight sharing. `L03, L06, L10, L12` · solid
- [[cortico-collicular-architecture]] — fast subcortical fusion modulated by slower cortical context. `L07` · developing
- [[cross-correlation-localisation]] — max dot product over shifts returns the ITD; full algorithm, no training. `L05, L09` · solid
- [[discrete-dynamic-neuron]] — recurrent weight μ_i plus delay Δ gives the unit a memory. `L02` · solid
- [[elastic-weight-consolidation]] — named as the regularisation example; no mechanism given. `L13` · stub
- [[evolutionary-algorithm]] — the scheme, the terminology mapping, and the four dialects that differ only in representation. `L11` · solid
- [[fitness-proportional-selection]] — Pr(i) = f_i / Σf_j — and its three failure modes, including transposition sensitivity. `L11` · solid
- [[gamma-gwr]] — GWR plus a context vector in the distance function — recurrence without a gradient. `L10, L13` · solid
- [[gated-multimodal-unit]] — learned convex combination of two modalities — the GRU gate, applied across senses. `L07` · solid
- [[gated-recurrent-network]] — input/forget/output gates; the additive cell state that fixes the gradient; superseded by attention in L04. `L03, L04, L07, L10` · developing
- [[genetic-inverse-kinematics]] — the genome is the answer; an unusually honest pros/cons table. `L11` · solid
- [[gpt]] — transformer decoder stack; attention instead of recurrence; generative pre-training. `L04, L09, L12, L13` · developing
- [[growing-dual-memory]] — GDM — episodic and semantic GWRs over a CNN; the module's most complete architecture. `L13` · solid
- [[gwr-network]] — grow a node when the input is poorly covered and the winner is well-trained; full algorithm. `L10, L13` · solid
- [[hinton-diagram]] — the weight matrix drawn as black and white squares; the oldest interpretability tool here. `L12` · solid
- [[histogram-based-som]] — SOM units store distributions, output likelihoods; reproduces the SC's measured signatures. `L07` · solid
- [[hodgkin-huxley-model]] — named once, as the thing the continuous model simplifies. `L02` · stub
- [[hubert]] — learning by listening; alternating clustering and masked prediction. `L04` · developing
- [[human-robot-collaboration]] — observe humans → model → implement → re-test; the module's only complete methodological loop. `L09` · developing
- [[hybrid-acoustic-tracking]] — correlate, predict with an SRN, turn the head; the three-stage robot. `L05` · solid
- [[hybrid-spiking-localisation-network]] — MSO/LSO/IC spiking front end plus a feed-forward read-out. `L05` · solid
- [[icub]] — named only as the gaze-shifting instructor in the collaboration study. `L09` · stub
- [[imitation-network]] — demonstrator/imitator stick figures; grounding words in one's own motor production. `L04, L08` · solid
- [[integrate-and-fire]] — leaky integration plus fire-and-reset with strong negative feedback. `L02` · solid
- [[jeffress-model]] — delay lines plus coincidence detectors; time becomes place. `L05` · solid
- [[layer-wise-relevance-propagation]] — conserved relevance pushed backwards to a per-pixel heatmap; the one XAI equation. `L12` · solid
- [[lime]] — explain a classifier locally by perturbing interpretable units. `L06` · developing
- [[locally-connected-network]] — local patches, private weights; plausible but worse. `L06` · developing
- [[mcculloch-pitts-neuron]] — the rate-coded unit; weighted sum, threshold, bias trick. `L02, L03` · solid
- [[multi-layer-associator]] — A1 ⇄ M1 chain trained by the Hebbian rule; the brain-shaped language model. `L04` · solid
- [[multi-layer-perceptron]] — layers of perceptrons; the resolution of XOR. `L03` · solid
- [[multichannel-cnn]] — MHI plus Sobel X and Y, three channels, 3D kernels — and no significant advantage. `L10` · solid
- [[nao]] — named only as the assistant platform with manipulated personality and autonomy. `L09` · stub
- [[neocognitron]] — Fukushima's S-cell/C-cell hierarchy; the link between cortex and the CNN. `L06` · solid
- [[neural-grasp-learning]] — the robot places an object to label its own grasps; self-supervision with a body. `L08` · solid
- [[neuroevolution]] — evolve weights, topology and hyperparameters at once — and every ingredient of the Baldwin effect. `L11` · solid
- [[nico]] — child-sized humanoid platform — two cameras, two microphones, haptic fingertips. `L08` · solid
- [[object-picking-architecture]] — visual hierarchy plus goal encoding; the modular↔end-to-end spectrum. `L08` · developing
- [[openpose]] — part confidence maps and part affinity fields, refined over stages, joined by bipartite matching. `L10` · solid
- [[preference-moore-machine]] — the formalism: continuous state vectors mapped to corner preferences. `L12` · solid
- [[progressive-neural-network]] — named as the dynamic-architecture example; no mechanism given. `L13` · stub
- [[ranking-selection]] — discard the values, keep the order; s ∈ [1,2] tunes pressure directly. `L11` · solid
- [[recurrent-neural-network]] — internal state; variable-length sequences. `L03` · solid
- [[roulette-wheel-selection]] — a sampler, not a distribution — high variance and a synchronisation barrier. `L11` · solid
- [[saliency-model]] — feature maps → centre–surround → normalise → saliency map → winner-take-all. `L09` · solid
- [[self-organising-map]] — Kohonen map; BMU, neighbourhood, topology-preserving unsupervised learning. `L04, L07, L08, L09, L10, L13` · solid
- [[simple-recurrent-network]] — BPTT by unrolling; used as L05's trajectory predictor. `L03, L05, L12` · solid
- [[snapshot-model]] — motion channel plus posture channel; complementary failure modes, stated honestly. `L10` · solid
- [[spike-response-model]] — kernel-based spiking model with exponential inhibitory feedback. `L02` · solid
- [[spiking-neural-network]] — the network built from spiking units; what temporal coding buys; first applied in L05. `L02, L05` · solid
- [[subsumption-architecture]] — layered task-achieving behaviours, each a complete robot; higher inhibits lower. `L08` · solid
- [[task-inference-network]] — self-organised network of behaviours for continual learning; a task code, not a label. `L08, L10` · developing
- [[tournament-selection]] — argmax over k random individuals; no global knowledge, pressure tuned by one integer. `L11` · solid
- [[transducer-network]] — the syntactic phrase-assignment RNN used as the worked extraction example. `L12` · solid
- [[weight-based-transfer]] — compiling symbolic rules into initial network weights. `L12` · solid
- [[word2vec]] — CBOW and skip-gram; embeddings learned from context. `L04, L10` · solid

## Entities

- [[adam-kendon]] — named twice, cited never — source of the continuum and the phases. `L10` · solid
- [[carl-wernicke]] — comprehension area, left superior temporal lobe; Wernicke's aphasia. `L04` · stub
- [[david-hubel]] — named via "Hubel and Wiesel"; simple and complex cells in striate cortex. `L06` · stub
- [[donald-hebb]] — originator of Hebb's rule; did not assume synaptic weakening. `L02` · stub
- [[kunihiko-fukushima]] — named via "Neocognitron (Fukushima)"; the middle link in the CNN's lineage. `L06` · stub
- [[leslie-ungerleider]] — named for the ventral/dorsal partitioning; no first name or co-author given. `L06` · stub
- [[lloyd-jeffress]] — named only via "Jeffress model"; predicted the delay-line mechanism in 1948. `L05` · stub
- [[paul-broca]] — empiricist counterpoint to phrenology; production area, Broca's aphasia. `L04` · stub
- [[rodney-brooks]] — named for the assumptions and the subsumption architecture; the module's only negative claim. `L08` · stub
- [[teuvo-kohonen]] — named only via "Kohonen map"; originator of the SOM. `L04` · stub
- [[torsten-wiesel]] — named via "Hubel and Wiesel"; attributed jointly throughout. `L06` · stub
- [[valentino-braitenberg]] — named only via “Braitenberg Vehicles”; no date or citation. `L08` · stub
- [[yann-lecun]] — named via "Convolutional NN (LeCun)"; the LeNet-5 diagram. `L06` · stub

## Syntheses

_(none yet)_

## Tag vocabulary

`agents` · `attention` · `biophysics` · `coding` · `competitive-learning` · `dynamics` ·
`embodiment` · `foundations` · `generalisation` · `geometry` · `grounding` ·
`history` · `imitation-learning` · `language` · `learning` · `localisation` ·
`multimodal` · `integration` · `perception` · `statistics` · `agents` · `attention` · `navigation` · `neural-networks` · `neuroscience` · `nlp` ·
`people` · `plasticity` · `rate-coding` · `recurrent` · `representation` ·
`audition` · `hybrid` · `localisation` · `robotics` · `self-organisation` · `self-supervised` · `speech` · `spiking` ·
`supervised` · `transformer` · `unsupervised` · `vision` ·
`continual-learning` · `memory` · `evaluation` · `development` · `symbolic` · `explainability` · `anatomy` · `architecture` · `contradiction` · `convolution` · `explainability` ·
`hierarchy` · `learning-rule` · `methods` · `plausibility` · `training` ·
`biology` · `evolution` · `gesture` · `optimisation`

## Not yet ingested

_Nothing — all 13 lectures ingested._
