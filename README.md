# THE MAINTENANCE PARADOX

### Understanding Is Not a Destination. It Is a Practice. And Neural Networks Just Proved It.

**ERI Labs · Jersey City, New Jersey · June 2026**

---

> *"We thought the problem was getting to understanding. It turns out the harder problem is staying there."*

---

## I. The Crystal Moment

There is a phenomenon that chemists call supersaturation. You take water. You heat it. You dissolve more salt into it than it could ordinarily hold. Then you let it cool, slowly, carefully, without disturbing it. What you get is a liquid that is technically more salt than water — a solution holding far more structure than it should be able to contain. It looks like water. It feels like water. But it is not water. It is water waiting.

Then you drop in a single grain of salt.

And in an instant — not gradually, not progressively, not over hours — crystals form. The entire solution reorganizes itself into a new structure. Order erupts from apparent disorder. The event takes milliseconds. The grain of salt is not the cause. It is the trigger.

Neural networks, it turns out, do exactly this.

For the first hundred thousand training steps — sometimes more — a neural network trained on a structured task looks exactly like a network that has learned nothing useful. Ask it to predict the answer to a modular arithmetic problem it has never seen, and it will be wrong as often as chance. It has memorized thousands of specific examples. It has driven its training loss to near zero. But it has not understood anything. It is, in the language of the researchers who study this phenomenon, supersaturated. It is holding more structure than it has yet organized.

Then, without warning, the crystal forms.

Test accuracy — the measure of how well the network handles problems it has never seen — jumps from near chance to near perfect. Not in days. Not over thousands of training steps. In a window so narrow that if you are not monitoring closely, you will miss it entirely. Researchers who discovered this transition in 2022 named it grokking, after a Robert Heinlein term for deep, sudden understanding. They could observe it. They could reproduce it. They could not, for two years, explain it.

This is the story of what it took to explain it — and of the stranger thing that happened when they did.

---

## II. The Problem With Understanding

Here is what everyone got wrong about grokking.

The natural assumption — the one that virtually every researcher in this field made — was that grokking was a destination. The network trained, the network struggled, the network arrived. Understanding was the end state. The job was to get there faster.

Anil Golechha, working through a series of experiments in 2024, confirmed that networks could grok without a particular regularization technique called weight decay. Weight decay works exactly as the name implies: it nudges all the network's parameters, at every training step, slightly back toward zero. It is a continuous, gentle erosion. The conventional wisdom said weight decay helped grokking happen faster. Golechha showed you could get there without it, though it took longer and worked less reliably.

The field accepted this and moved on. Weight decay was a helpful accelerant, not a requirement.

Then, in February 2026, Harsh Prakash and Charles Martin posted a paper that changed the question.

They had been running networks past grokking — far past it, into training regimes that no one had previously thought to examine. Millions of steps beyond the generalization event. And what they found was this: the networks forgot.

Not gradually. Not because new data had arrived to overwrite what they knew. Not because the architecture was too small. The networks had perfect training accuracy — they remembered every example they had been shown — and they lost their ability to generalize entirely. Test accuracy, which had been near perfect, collapsed. The crystal, after millions of steps of perfect solidity, had dissolved back into solution.

Prakash and Martin named this anti-grokking. They documented it meticulously. They identified the only early warning sign they could find: a class of outlier eigenvalues in the weight matrices that they called Correlation Traps, whose accumulation preceded the collapse by millions of steps. They confirmed that the phenomenon appeared specifically in networks trained without weight decay.

They ruled out every obvious explanation. Not overfitting. Not catastrophic forgetting. Not a technical artifact. Something real was dissolving, and they did not know what.

The field had a new problem. Understanding, it turned out, was not a destination. It was a state that required maintenance — and no one knew why, or what the maintenance was.

---

## III. What Understanding Actually Is

To understand what was dissolving, you need to understand what understanding actually is — not in the poetic sense, but in the geometric one.

Every neural network with $n$ parameters exists in a parameter space of dimension $n$. For a modern language model, $n$ might be billions. For the small modular arithmetic networks where grokking was first studied, $n$ is in the thousands. The training process is a walk through this space, guided by gradients.

Now consider the Fisher information matrix. It is a mathematical object that tells you, at any point in parameter space, which directions actually matter. Formally, it is the expected outer product of the gradient of the log-likelihood — a measure of how sensitively the network's predictions change when you perturb its parameters in each possible direction. If moving along a particular direction in parameter space changes the network's predictions substantially, that direction has high Fisher information. If moving along a direction leaves the predictions unchanged, that direction has zero Fisher information — it lies in what mathematicians call the kernel of the Fisher matrix.

Here is what the Fisher matrix looks like inside a network that is memorizing but not understanding.

Almost everything is in the kernel. Almost every direction in parameter space — thousands of dimensions, tens of thousands — produces no meaningful change in the network's predictive behavior. The network has learned to map training examples to correct answers, but the map is not systematic. It is a lookup table, not an algorithm. The Fisher matrix is nearly rank-zero: a vast null space with almost nothing carrying real signal.

And here is what it looks like after grokking.

A small subspace — somewhere between two and seventeen percent of the full parameter dimension — has crystallized into a coherent information-bearing structure. The researchers who identified this structure call it $\mathrm{col}(F)$: the column space of the Fisher matrix. It is the part of parameter space that is carrying genuine information about the data distribution, as opposed to memorized noise. And crucially, it is stable. It maintains itself. The parameters in this subspace are organized around a real signal, and that organization persists under continued training.

The grokking event is not a jump in accuracy. It is a jump in rank.

This is what crystallizes: not performance, but geometric structure. The network's parameter space reorganizes itself around a small, coherent, information-rich subspace — the same way that a supersaturated solution reorganizes itself around a crystal seed. One moment there is disorder. The next there is structure. And the jump in test accuracy is simply the visible consequence of that geometric reorganization.

The scalar that tracks this — the one number you would want to monitor if you were watching a network grok in real time — is $r/n$: the fraction of parameter dimensions carrying active Fisher information. Below $0.01$, the network is in the null sector, memorizing but not understanding. Between $0.02$ and $0.17$, the crystal has formed. The network understands.

When Prakash and Martin's networks anti-grokked, $r/n$ was doing something precise: it was falling back toward zero. The crystal was dissolving. The column space was collapsing back into the kernel. The network was not forgetting its training examples — those were still perfectly memorized. It was losing the geometric structure that made generalization possible. The crystal was returning to solution.

---

## IV. The Paradox of Erasure

Now comes the part that no one predicted.

Recall that weight decay is the technique of nudging all parameters, at every step, slightly back toward zero. It sounds, described this way, like a mechanism for forgetting. You are continuously erasing the network's learned parameters. You are making the weights smaller. You are, in some literal sense, reducing what the network has accumulated.

And yet: networks trained with weight decay do not anti-grok. The crystal, once formed, holds.

The explanation took a detour through a 100-year-old problem in pure mathematics.

In 1920, Srinivasa Ramanujan wrote his last letter to G. H. Hardy from a sickbed in Madras. In it, he described seventeen functions he had discovered that behaved almost like a class of well-understood mathematical objects called theta functions — but not quite. They were structurally similar but violated one key property. They were, Ramanujan wrote, mock theta functions. He gave no proofs. He died two months later.

For eighty years, mathematicians knew how to work with mock theta functions but could not explain what they were. They sat at the edge of known theory, coherent but homeless. In 2002, Sander Zwegers, a doctoral student in Utrecht, solved the problem. Mock theta functions, he showed, were incomplete objects. Each one could be completed by appending a specific correction — a non-holomorphic shadow — that transformed it into a proper, well-behaved mathematical structure called a harmonic Maass form. The shadow was not decorative. It was structural. Without it, the mock theta function remained partial, bounded, unable to transform correctly under the operations that defined the theory.

The key insight: the shadow is not a one-time addition. It is an ongoing correction. Remove it, and the completed object degrades back into a mock theta function.

Weight decay, in the Fisher rank framework, is the shadow.

The column space $\mathrm{col}(F)$ — the crystallized information-bearing subspace — is not self-sustaining under stochastic gradient descent without regularization. Gradient noise continuously bleeds parameter mass from the signal-bearing directions into the null sector. Over millions of steps, this erosion is sufficient to dissolve the crystal entirely. Weight decay counteracts this: by continuously contracting all parameters back toward zero, it removes the accumulated noise from the null sector at each step, maintaining the boundary between what carries information and what does not.

Grokking without weight decay is therefore not the same event as grokking with weight decay. In the absence of the shadow operator, the crystal can still form — noise fluctuations at the transition can accidentally project enough structure into the column space to trigger crystallization. But the crystal is unstable. It will dissolve. Not in days, but in millions of training steps.

Anti-grokking is not a failure. It is an unmaintained crystal.

---

## V. The Maintenance Paradox

This is the new framework. Not grokking. Not anti-grokking. The Maintenance Paradox.

Understanding — in neural networks, and possibly in more general systems — is not a state that, once achieved, persists by default. It is a state that requires continuous expenditure of something in order to remain. For neural networks, that something is weight decay: the structured, continuous erasure of accumulated noise. The erasure is not destroying understanding. The erasure is what keeps understanding from destroying itself.

This inverts the intuition. The natural model of learning looks like this: effort leads to understanding, and understanding, once achieved, is the payoff. The hard part is getting there. Staying there is automatic.

The Maintenance Paradox says: no. Getting there is hard. Staying there is also hard, in a different way. And the mechanism that keeps you there looks, on the surface, like the thing that would make you forget.

The Maintenance Paradox has a timescale. The mathematics here — drawn from a theorem proved by Ángel Capel and colleagues in June 2026, concerning how quickly probability distributions on curved spaces reorganize themselves — predicts that the dissolution process takes roughly:

$$T_{\text{anti}} \sim \frac{\dim(M)^{4/3}}{\rho_{\text{LSI,post}}}$$

Two things matter here. First: the $4/3$ exponent, which means the dissolution timescale grows faster than the model's dimension. Larger networks resist dissolution longer, not because they understand better, but because they have more geometric inertia. Second: $\rho_{\text{LSI,post}}$ — a measure of the curvature of the loss landscape after grokking — is much larger than the corresponding value before grokking. This means dissolution is far slower than crystallization. Networks fall into understanding quickly and dissolve from it slowly. The crystal forms in a flash. It takes millions of steps to melt.

This is exactly what Prakash and Martin observed.

---

## VI. The Memory in the Void

But the Maintenance Paradox has one more chapter, and it may be the most surprising of all.

When a network anti-groks — when its $r/n$ falls back toward zero and the crystal dissolves — it does not return to where it started. This is the part that has not yet been confirmed experimentally but that falls out of the mathematics with considerable force.

Before the first grokking, the network's null sector — the kernel of the Fisher matrix, the vast space of directions carrying no generalization signal — is generic. It is shaped by the random initialization and the accumulated noise of memorization. It does not encode anything about the structure of the problem.

After anti-grokking, the null sector is different. It has been shaped by the geometry of the crystallized state that came before it. The directions that were in $\mathrm{col}(F)$ during the grokking phase have left a geometric imprint on the null sector that surrounds them. The kernel is not empty. It is, in some precise mathematical sense, pre-shaped for the next crystallization.

The prediction — untested as of June 2026 — is that if you reintroduce weight decay after anti-grokking, the network will grok again, and faster. Not because it explicitly remembers what it learned. But because the void it fell into is not the same void it started in. The crystal has left a mold in the solution.

The salt crystal dissolved. But the solution that remains is not the same water you started with.

---

## VII. The Wider Implication

There is a habit, in thinking about intelligence — human or artificial — of treating understanding as the end of the story. You struggle. You practice. You reach the moment of insight. And then you have it.

What the Fisher rank framework suggests is that this story is incomplete. Understanding is a crystallized state in a space that is always under thermal pressure. The pressure is gradient noise, parameter diffusion, the continuous small perturbations of any learning process that continues after the insight has arrived. Without something that actively maintains the structure — weight decay in networks, perhaps practice and retrieval in humans, active accretion in astrophysical systems — the crystal dissolves. Not all at once. Slowly, over timescales that depend on how large the system is and how sharply curved the landscape around understanding happens to be.

The question this raises for artificial intelligence is immediate: every large language model deployed at scale is, in some sense, past its grokking events. Every capability it has crystallized was crystallized during training. If the Maintenance Paradox applies at scale — and the domain-specific version of the prediction says it should — then there are domains within large models where the crystal has already begun to dissolve, where per-layer diagnostic signals are falling below the healthy threshold, while the aggregate benchmark numbers still look fine.

The HTSR quality exponent $\alpha$, developed by Martin and Mahoney over a decade of work on weight matrix spectral analysis, can detect this. The Fisher rank fraction $r/n$ can detect it. Per-layer monitoring, run continuously against deployed models, would tell you not just whether the model is performing but whether the understanding it is performing with is structurally intact.

We have never built that monitoring infrastructure. We have, implicitly, assumed that crystallization is permanent. The February 2026 experiments suggest it is not.

---

## VIII. What the Crystal Teaches

Back to the supersaturated solution.

When the crystal forms — when the grain of salt drops into the waiting water and structure erupts — what has actually changed? The number of salt molecules in the solution has not changed. The temperature has not changed. The total energy of the system has not changed. What has changed is the organization of that energy: the same atoms, now arranged into a pattern that is self-reinforcing, that propagates itself, that takes a disordered system and locks it into a state of lower entropy and higher structure.

The crystal is not made of new atoms. It is made of the same atoms, organized.

And the crystal dissolves not because the atoms change but because the conditions that organized them are no longer maintained. Temperature rises. Agitation increases. The organizing force withdraws. And the structure that looked so solid, that had looked permanent, returns to the disorder it temporarily escaped.

For neural networks, the organizing force is weight decay. For the Fisher matrix, the crystal is $\mathrm{col}(F)$. For understanding — of any kind, in any system — the question is always the same: what is the mechanism maintaining the organization? What is the shadow operator? What, if withdrawn, would let the crystal dissolve?

The answer is not always obvious. It is almost never automatic.

Understanding is not a destination.

It is a practice.

---

## Open Questions

The following predictions follow from the Maintenance Paradox framework and have not been tested as of June 2026:

**Q1 — The Second Crystal**: Re-introducing weight decay after anti-grokking should produce faster re-crystallization than the first grokking event, because the post-dissolution null sector retains geometric memory of the first crystal. Testable with standard grokking experimental infrastructure.

**Q2 — The Domain Gradient**: In large language models, anti-grokking should be domain-specific and asynchronous — mathematical capabilities dissolving before linguistic ones, or vice versa, depending on domain-level log-Sobolev constants. Per-layer HTSR monitoring would detect this before benchmark decline.

**Q3 — The Scale Prediction**: Anti-grokking onset should scale as $\dim(M)^{4/3}$ — meaning larger models anti-grok later, in ways that diverge predictably from smaller models at the same learning rate.

**Q4 — The Optimal Dose**: There exists, for each domain in a large language model, an optimal weight decay $\lambda_c$ that maintains the Fisher crystal without over-erasing it. That optimum scales with domain difficulty. Too much erasure collapses the crystal; too little allows it to dissolve.

---

## References

1. Power, A. et al. (2022). Grokking: Generalization Beyond Overfitting on Small Algorithmic Datasets. *arXiv:2201.02177*
2. Golechha, S. (2024). Progress Measures for Grokking in Real-World Tasks.
3. Prakash, H. K. & Martin, C. H. (2026). Late-Stage Generalization Collapse in Grokking. *arXiv:2602.02859*
4. Prakash, H. K. & Martin, C. H. (2026). Grokking and Generalization Collapse: Insights from HTSR Theory. *arXiv:2506.04434*
5. Li, Z. et al. (2026). Where to Find Grokking in LLM Pretraining? *arXiv:2506.21551*
6. Capel, Á. et al. (2026). Rapid Mixing for Gibbs Measures in Riemannian Manifolds. *arXiv:2606.13453*
7. Martin, C. H. & Mahoney, M. W. (2021). Implicit Self-Regularization in Deep Neural Networks. *JMLR*, 22(165).
8. Zwegers, S. P. (2002). *Mock Theta Functions*. Doctoral thesis, Universiteit Utrecht.
9. Amari, S. (1998). Natural Gradient Works Efficiently in Learning. *Neural Computation*, 10(2).
10. Kirkpatrick, J. et al. (2017). Overcoming Catastrophic Forgetting in Neural Networks. *PNAS*, 114(13).

---

*ERI Labs · Jersey City, New Jersey · June 2026*
