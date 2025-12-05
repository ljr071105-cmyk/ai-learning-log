# PRETRAINING 
###### ***<span style="background:rgba(173, 239, 239, 0.55)">Base model  "Internet document simulator"</span>***
## <span style="background:#d2cbff">step1: Download and preprocess the internet</span>

![](Pasted image 20251125133226.png)
* **URL Filtering**: where you don't want to be getting data from
* **Text Extraction** : just want text of this web page
* **Language Filtering** : what fraction of all different types of language are we going to include in our data set
## <font color="#000000"><font color="#000000"><span style="background:#d2cbff"><font color="#000000">step2: Toke</font>nization</font></span></font>
 text --> bits --> bytes --> tokens
 <span style="background:#fff88f">bite pair encoding algorithm</span> : iteratively merge the most common token pair to mint new token
![](Pasted image 20251125135845.png)
## <span style="background:#d2cbff">step3: Neural network training</span>
![](Pasted image 20251125140421.png)## Prompt 1
Your task is to create HTML and CSS files for a stylish form offering 15% off to new subscribers. The product is called BarkerBox, a monthly subscription box that brings an assortment of unique, playful, and durable toys that dogs will love. 

Please follow the steps below:
 * Create a heading for the product name and offer.
 * Add a short paragraph describing the product and the benefits of subscribing.
 * Build a registration form with a required email input field and a “Get My 15% Off” button.
 * ​​Use the color #471869 for the heading and button, a white background for the form container, and a light body background.

Keep the code as beginner-friendly as possible and document each CSS property with a short code comment to make the code more understandable for non-coders.## Prompt 1
Your task is to create HTML and CSS files for a stylish form offering 15% off to new subscribers. The product is called BarkerBox, a monthly subscription box that brings an assortment of unique, playful, and durable toys that dogs will love. 

Please follow the steps below:
 * Create a heading for the product name and offer.
 * Add a short paragraph describing the product and the benefits of subscribing.
 * Build a registration form with a required email input field and a “Get My 15% Off” button.
 * ​​Use the color #471869 for the heading and button, a white background for the form container, and a light body background.

Keep the code as beginner-friendly as possible and document each CSS property with a short code comment to make the code more understandable for non-coders.
 predict, update, optimize
#### neural network internals
![](Pasted image Pasted image 20251125141031.png)
like knobs on a DJ set and as you're twiddling these knobs, you're getting different predictions for every possible token sequence input
#### inference: to generate data, just predict one token at a time
![](Pasted image Pasted image 20251125141902.png)
eg. run the Llama
    the "psychology" of a base model
<ol>
<li>It is a token-level internet document simulator</li>
<li>It is stochastic / probabilistic : you're going to get something else each time you run</li>
<li> It "dreams" internet documents</li>
<li>It can also recite some training documents verbatim from memory ("regurgitation")</li>
      eg.an Assistant that answers questions using a prompt that looks like a conversation
   </ol>
# POST-TRAINING: SUPERVISED FINETUNING
###### *<span style="background:rgba(173, 239, 239, 0.55)">SFT model  An assistant, trained by Supervised Finetuning </span>
## <span style="background:#d2cbff">Conversations</span>
![](Pasted image 20251127111843.png)
-  training <font color="#c00000">implicitly</font> through neural networks based on data sets of conversations
![](Pasted image Pasted image 20251127120134.png)
* Human labelers write conversations based on labeling instructions
* today, a huge amount of labeling is LLM assisted
     eg.humans edit more than write, or just synthetic 
## <span style="background:#d2cbff">Conversation Protocol / Format</span>
### Tiktokenizer
![](Pasted image 20251127112045.png)
## <span style="background:#d2cbff">Hallucinations</span>
#### Mitigation #1
=> Use model interrogation to discover model's knowledge, and programmatically augment its training dataset with lnowledge-based refusals in cases where the model doesn't know
![](Pasted image 20251127150619.png)
#### Mitigation #2
=> Allow the model to search
![]Pasted image 20251127150717.png|425)

## <span style="background:#d2cbff">Congnitive Deficits</span>
##### 1."Vague racollection" VS "Working memory"
* Knowledge in the parameters == Vague recollection
    eg. like something you read 1 month ago
* Knowledge in the tokens of the context of the context window == Working memory

##### 2.Knowledge of self
The LLM has no knowledge of self"out of the box"
if you do nothing, it will probably think it is like ChatGPT, developed by OpenAI,
You can program a "sense if self" in 2 ways:
1. hardcoded conversations aroud these topics in the Conversation data.
2. "system message" that raminds the model at the beginning of every conversation about ite identity.

##### 3.Models need tokens to `think`
![](Pasted image 20251127155925.png)
the right answer is better, because it distributes your competition across many tokens, ask models to <font color="#c00000">create </font><font color="#c00000">intermediate results</font> or whenever <font color="#c00000">you can(and should) lead on tools and tool use</font> like Python interpreter "Use code" or Web search.
![](Pasted image 屏幕截图 2025-11-27 155630.png)
##### 4.Others
1. Models can't count
2. Models are good with spelling
    remember they see tokens(text chunks), not individual letters  

# POST-TRAINING: REINFORCEMENT LEARNING
![](Pasted image 屏幕截图 2025-11-28 210252.png)
#### eg1.deepseek : thinking models -- chains of thoungths

![](Pasted image Pasted image 20251130100744.png)

* in the process of problem sovling: re-evaluating steps it has learned that it works better for accuracy
* manipulate a problem and how you approach it from<font color="#c00000"> diffrenet perspectives</font>
* how you pull in some analogies or do different kinds of things like that
* how you kind of try out many different things over time, check a result from different perspectives
* <font color="#c00000">there is no human who can hardcode this stuff in the idea assistant response, this is only something that can be discovered in the process of reinforcement learning</font>
#### eg2.the system AlphaGo
![](Pasted image Pasted image 20251130104105.png)
* supervised learning:
    * imitating
    * top out and nerver quite better than some of the top top players
*  reinforcement learning:
```
    * agianist itself
```
    * can do significantly better and overcome even the top players
### Reinforcement Learning in un-verifiable domains
==> PLHF(Reinforcement Learning from Human Feedback)
<span style="background:rgba(173, 239, 239, 0.55)">RL model</span>
![](Pasted image Pasted image 20251130111525.png)
##### Naive approach:
* run RL as usual of 1,000 updates of 1,000 prompts of 1,000 rollouts (cost 1.000,000,000 scores from human)
##### RLHF approach:
* Step1: Take 1,000 prompts, get 5 rollouts, order them from best to worst (cost 5,000 scores from humans)
* Step2: Train a neural net simulator of human preferences ("reward model")
* Step3: Run RL as usual, but using the simulator instead of actual humans
######  RLHF upside
* We can run RL in arbitrary domains (even the unverifiable ones)
* This (empirically) improves the performance of the model, possibly due to the "discriminator - generator gap"
* In many cases, it is much easier to discriminate than to generate
* eg. "Write a poem" vs. "Which of these 5 poems is best?"
###### RLHF downside
* We are doing RL with respect to lossy simulation of humans, it might be missleading
* Even more subtle: RL discovers ways to "game" the model
* eg. after 1,000 updates, the top joke about pelicans is not the banger you want, but something totally non-sensical like "the the the the the"

![](Pasted image e2ef256727be60db14f63836fa939761.png)

![](Pasted image d8b52d9e6c7d32f783a3acf47c1c05cd.png)