### Master Thesis: Measuring Aspect-Oriented Software In Practice

---

**Acknowledgements**. I have thought that the acknowledgements are a very valuable part to begin the thesis text, since all the expressed thoughts must reach to the right people before delving into it. You will only see me while reading the whole thesis, whereas there are a few invaluable people who deserve certainly my thanks –without them I would never get the fruits of my work. 
 
First of all, many thanks to my supervisor, Tim Molderez for his continuous guidance, support and encouragement throughout the year that make this work possible. Also, my thanks go to my promoter, Prof. Dr. Dirk Janssens for initiating this work in order to be enlightened some missing parts of this area. I would like to thank my family for their continuous support and encouragement. Although they are far from me, I felt them next to me while working on it.

Finally, I would like to dedicate this thesis to *my mother* and end up with a quote of Hz. Mevlâna (Rumi) that summarizes my one-year effort:
 
> *Patience is the key to joy.*
 
**Abstract**. Aspect-oriented programming (AOP) is a way of modularizing a software system by means of new kind of modules called aspects in software development. To this end AOP helps in alleviating crosscutting concerns of system modules by separating into several aspect modules, thereby aiming to improve separation of concerns. On the other hand, aspects can bring unexpected behaviour to a system while attempting to alter the system’s concerns. They can modify the behaviour of the base system without warning. Following to this, such impact can limit to achieve modular reasoning in an aspect-oriented system properly.

Obtaining the valuable data, we try to get an idea of how difficult it is to achieve modular reasoning. In this thesis, we analyse the existing ten **[AspectJ](http://eclipse.org/aspectj/)** systems by answering six research questions. These six questions were derived from our general question: *"how AspectJ is used in practice?"*. In order to answer each one of them, we have implemented
a metrics suite including both **[aspect-oriented ](http://en.wikipedia.org/wiki/Aspect-oriented_programming)** and **[object-oriented ](http://en.wikipedia.org/wiki/Object-oriented_programming)** features using [**Ekeko**](https://github.com/cderoove/damp.ekeko). Next to modular reasoning, we also acquire other usefulness about AOP constructs and coupling between classes and aspects. These results can then be used to influence the design of existing or new AOP languages, or to improve existing analysis tools of AOP.

### Hierarchy of the research questions 
---

1.	How large is the system?
	*	*Lines of Code* (LOC)
	*	*Vocabulary Size* (VS)
	*	*Number of Attributes* (NOA) 
	*	*Number of Methods* (NOM)
2.	How often are AOP constructs used compared to OOP features?
	*	*Number of Intertypes* (NOI)
	*	*Number of Advice* (NOAd)
3.	Which AOP constructs are typically used?
	*	*Inherited Aspects* (InA)
	*	*Singleton Aspects* (SA)
	*	*Non-Singleton Aspects* (nSA)
	*	*Advice-Advanced Pointcut Dependence* (AAP)
	*	*Advice-Basic Pointcut Dependence* (ABP)
	*	*Number of Around Advice* (NOAr)
	*	*Number of Before/After Advice* (NOBA)
	*	*Number of After Throwing/Returning Advice* (NOTR)
	*	*Number of Call* (NOC)
	*	*Number of Execution* (NOE)
	*	*Adviceexecution-Advice Dependence* (AE)
	*	*Number of Wildcards* (NOW)
	*	*Number of non-Wildcards* (NOnW)
	*	*Argument size of Args-Advice* (AAd)
	*	*Argument size of Args-Advice-args* (AAda)
4.	How many types and members of a system are advised by AOP?
	*	*Percentage of Advised Classes* (AdC)
	*	*Percentage of non-Advised Classes* (nAdC)
	*	*Number of Advised Methods* (AdM)
	*   *Number of non-Advised Methods* (nAdM)
	*	*Classes and Subclasses* (CsC)
	*	*Average of Subclasses of Classes* (ScC)
5.	Is there a connection between the amount of coupling in an aspect, and how many shadows it advises?
	*	*Advice-Join Point Shadow Dependence* (AJ)
	*	*Number of thisJoinPoint/Static* (tJPS)
	*	*Number of Modified Args* (MoA)
	*	*Number of Accessed Args* (AcA)
	*	*Around Advice - non-Proceed Call Dependence* (AnP)
6.	How many dependencies are there between classes and aspects?	
	*	*Attribute-Class Dependence* (AtC)
	*	*Advice-Class Dependence* (AC)
	*	*Intertype-Class Dependence* (IC)
	*	*Method-Class Dependence* (MC)
	*	*Pointcut-Class Dependence* (PC)
	*	*Advice-Method Dependence* (AM)
	*	*IntertypeMethod-Method Dependence* (IM)
	*	*Method-Method Dependence* (MM)
	*	*Pointcut-Method Dependence* (PM)


### Have a look at an example
---
One of the questions we examine is: how many aspects extend to an abstract aspect in a given aspect-oriented project?

The Metric representation of the question is: the number of inherited aspects in a given aspect-oriented project.

```Clojure
 (defn NOInheritedAspects [?aspectname ?abstractname]
         (l/fresh [?aspect ?source ?super]
                   (NOAspects ?aspect ?source)
                   (w/aspect-declaredsuper ?aspect ?super)
                   (equals ?aspectname (str "Aspect {"(.getSimpleName ?aspect)"}"))
                   (equals ?abstractname (str "From Abstract Aspect -> "(.getSimpleName ?super)))
                   (succeeds (.isAbstract ?super))))
```
### Selected aspect-oriented systems
---

1. [HealthWatcher](http://www.kevinjhoffman.com/tosem2012/)
2. HyperCast
3. [AJHotDraw](http://ajhotdraw.sourceforge.net/)
4. [AJHSQLDB](http://sourceforge.net/projects/ajhsqldb/)
5. [Contract4J5](https://github.com/deanwampler/Contract4J5)
6. [MobileMedia](http://sourceforge.net/projects/mobilemedia/)
7. [iBatis](sourceforge.net/projects/ibatislancaster/)
8. [Telestrada](http://www.kevinjhoffman.com/tosem2012/)
9. SpaceWar
	* Comes bundled with the [AJDT](http://eclipse.org/ajdt/)
10. [TetrisAJ](http://www.guzzzt.com/coding/aspecttetris.shtml)

### Some charts mentioned in this thesis
---
All the related charts are available in [Thesis Text/diagrams](https://github.com/ozlerhakan/aop-metrics/tree/master/Thesis%20Text/diagrams).

### How the metrics work
---

First of all, make sure that you have all the dependencies about the *Ekeko plug-in* in your latest [Eclipse IDE](https://www.eclipse.org/downloads/) (currently it is Neon.2 4.6), if not, you first need to download the dependencies:

  * [AspectJ Development Tools](https://eclipse.org/ajdt/downloads/) 
  	* Just look at the AJDT dev builds for the latest Eclipse IDE and download it via ```Help > Install New Software...``` by pasting its update site URL (e.g. http://download.eclipse.org/tools/ajdt/46/dev/update for 4.6 version)   
  * AST View [i.e. org.eclipse.jdt.astview](http://www.eclipse.org/jdt/ui/astview/index.php)
  	* Install AST view for Eclipse 4.4 and later version via ``Install New Software``
  * [Counterclockwise](http://doc.ccw-ide.org/documentation.html#install-as-plugin)
  	* Counterclockwise is available via the Eclipse Marketplace Client: search for Counterclockwise

Downloading the dependencies, you are now ready to install the prebuilt *Ekeko* plug-in:

  * Go to: ```Help > Install New Software...``` in your Eclipse IDE.
  * Copy and paste this url: http://soft.vub.ac.be/~cderoove/eclipse/ in the Work with text field.
  * Hit Enter.
  * Select Ekeko-based Program Analysis including [Ekeko](https://github.com/cderoove/damp.ekeko) and [GASR](https://github.com/cderoove/damp.ekeko.aspectj) (*required*), the rest are (*optional*) and install them.
  * After installing both Ekeko and the Ekeko's AspectJ extension, you are ready to downdload the metrics.
  * Import the project (i.e. the aopmetrics project) in your Eclipse workspace. Drag and drop it on the package explorer.
  * Select an AspectJ project (e.g. SpaceWar) that you want to analyse then, right-click on the project on the package exlorer, apply these steps: ```Configure > Include in Ekeko Queries```
  * **The metrics also need soot analyses in order to run properly**. To do that, we need to configure the selected AspectJ project once as follows:
      *  Right-click on the project : ```Properties > Ekeko properties```.
      *  Click the Select button and now choose the class that contains the main() method of the AspectJ project. (e.g. you can find an example of a main() method in our AJTestMetrics project ([MainTST](https://github.com/ozlerhakan/aop-metrics/blob/master/AJTestMetrics/src/ua/thesis/test/MainTST.java)).
		* If your AspectJ project does not have a main() method, just **create** a new one to set it as an execution point of soot analysis.
      *  Write the following line into the "Soot arguments:" one-line text box: ```-no-bodies-for-excluded -src-prec c -f jimple -keep-line-number -app -w -p cg.cha```
      * Click OK
      * Finally, ```right-click the project > Configure > Enable Ekeko Soot Analyses```.

  * Activate an Ekeko-hosted REPL by doing ```Ekeko > Start nRepl``` from the main Eclipse menu. A dialog shows the port on which the nRepl server listens (e.g. ```nrepl://localhost:51721```)
  * Connect to the Ekeko-hosted REPL: Go to: ```Window > Connect to REPL``` to connect to this port (i.e. ```nrepl://localhost:51721```). A Counterclockwise REPL view now opens.
  * There are 2 steps to run the metrics:
 	* First,  Open the ```aopmetrics/metrics.clj``` file and right-click somewhere on the file and choose ```Clojure > Load file in REPL ```. After obtaining ```#'aopmetrics.metrics/AcA``` in the REPL, follow the second step.
	* Second, Open the ```aopmetrics/core.clj``` file and right-click somewhere on the file and choose ```Clojure > Load file in REPL ```.
		* You will see ```nil``` in the REPL which means that everything goes correctly and the metric framework has likewise been loaded in the REPL.
  	* Third, again right-click somewhere on the file and choose ```Clojure > Switch REPL to FILE's namespace```.
		* You will see something similary like ``#object[clojure.lang.Namespace 0x19805029 "aopmetrics.core"]`` in the REPL.
  		* After disabling the comment block, you can now run the metrics. For example, if we look at ```(metrics/VSize)```, the alias name of our ```AOPMetrics``` is ```metrics``` that helps in reaching the implemented metrics in a short way rather than typing the totally qualified name (i.e. ```AOPMetrics```). Type ```(metrics/Vsize)``` on the REPL and it simply retrieves the vocabulary size of the project that you enabled Ekeko Queries.

---

:pushpin:**Note:** The AcA and MoA metrics need different soot arguments to obtain the exact data. Thus, you need to change the current arguments with the following one: ```-no-bodies-for-excluded -src-prec c -f jimple -keep-line-number -app -w -p jb use-original-names:true -p cg.cha``` and run again ```Ekeko Soot Analyses```.


>:exclamation:**One Potential Issue**: There was an encountered issue about soot analysis. If you get the same problem called ```RuntimeException : tried to get nonexistent method```, while attempting to run the metrics especially for the AM, IM, and MM metrics.  You can find more information on it from https://github.com/ozlerhakan/aop-metrics/issues/1 

### License 

Copyright © 2014 Hakan Özler.

