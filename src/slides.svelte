<script lang="ts">
	import { Presentation, Slide, Code} from '@components'
	import Layout from './layout.svelte'
	import Stack from '@lib/components/stack.svelte'
	import Step from '@lib/components/step.svelte'
</script>

<Presentation>	
	<script>
		// information about this presentation 
		const author = "Anran Wang"; 
		const title = "Necessary Liberal Preconditions";
		const subtitle = "A Proof System";
		const department = "Department of Informatics"; 
		const university = "Technical University of Munich"; 
		var currentChapterName="";
		var currentChapterNumber=0;
		var chapterNames=[];
		var toc = false;  // whether toc is to be generated
		// data structure: [chapter number, chapter name, slideID]
		var presentationData = []; 
		function newChapter(chapterName){
			currentChapterName=chapterName; 
			currentChapterNumber++;
			chapterNames.push(chapterName);
		}
		// called when new slide is created
		var page=0; 
		function newPage(){
			page++; //note that page n has index n-1
			presentationData.push({chapternr: currentChapterNumber, chapter:currentChapterName,pagenr:page});
		}
	</script>

	<!-- title page -->
	<Slide><Layout>
		<div class="m-16 flex h-[25vh] w-[90vw] bg-[var(--themecolor)] text-white items-center justify-center gap-[100px]">
			<div>
				<div class="text-[6vh]">
					<span id="mytitle"> </span>
				</div>
				<br>
				<div class="text-[4vh]">
					<span id="mysubtitle"> </span>
				</div>
			</div>
		</div>
		<br>
		<div> 
			<span id="myname"></span>
		</div>
		<br>
		<div class="text-[3vh]"> 
			<span id="myuni"></span>
		</div>
		<div class="text-[3.5vh]"> 
			<br>
			<span id="mydate"></span>
		</div>
		<br>
		<div class="flex items-center justify-center">
			<img class="h-[10vh] align-middle" src="/ma-slides/tum-logo.svg" alt="tum logo">
		</div>
		<!-- fill in the data for this presentation  -->
		<script> 
			document.getElementById("mytitle").innerHTML=title;
			document.getElementById("mysubtitle").innerHTML=subtitle;
			document.getElementById("myname").innerHTML=author;
			document.getElementById("myuni").innerHTML=department+"<br>"+university;
			let today = new Date(); 
			document.getElementById("mydate").innerHTML=today.toISOString().split('T')[0]; 
		</script>
	</Layout></Slide>
	<!-- TOC -->
	<Slide>
		<script>
			toc=true; // decides whether to generate table of content page
		</script>
		<Layout>
			<toc class="flex h-full items-center justify-center gap-[100px]">
				<chpicons> </chpicons>
				<chpnames class="text-left"> </chpnames>
			</toc>
		</Layout>
	</Slide>
	
	<!-- intro -->
	<script>newChapter("Motivation");</script>
	<Slide animate><Layout>
		<titlebar>Why proof?</titlebar>
		<mybody>
			<Step fadeIn>
				<div>
					<img src="https://facereader.witbacon.com/assets/img/sunzi.jpg" alt="wlp_d" class="w-[50vw] ">
					<div>Awesome! </div>
				</div>
			</Step>
			<Step fadeIn>
				<div>
					<ul class="a">
						<li> Provided....</li>
						<li> and.... </li>
						<li> then.... </li>
					</ul>
				</div>
			</Step>
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar>Hoare Triple</titlebar>
		<mybody>
			<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[40vw]">
				<Code lang="java" lines= "1-2" class="bg-[#163468]"> 
					{`
						action := writeThesis;
						thesis := written; 
					`}
				</Code>	
			</div>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>Hoare Triple</titlebar>
		<mybody>
			<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[60vw]">
				<Code lang="java" lines= "2,4|1,2,4|1-4|1-5"> 
					{`
					 {action == playGame, thesis == unwritten}
					 action := writeThesis;
					 {action == writeThesis, thesis == unwritten}
					 thesis := written; 
					 {action == writeThesis, thesis == written}
					 `}
			</Code>	
			</div>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>Weakest Precondition</titlebar>
		<mybody>
			<div>
				<Step fadeIn>
					{`{action == doDishes}`}
				</Step>
				<Step fadeIn>
					{`{action == takeHike}`}
				</Step>
				<Step fadeIn>
					{`{action == sleepIn}`}
				</Step>
				<Step fadeIn>
					{`{thesis == written}`}
				</Step>
				<br>
				<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[60vw]">
					<Code lang="java" lines= "1-5" > 
						{`
						{action == ?, thesis == ?}
						action := writeThesis;
						{action == writeThesis, thesis == ?}
						thesis := written; 
						{action == writeThesis, thesis == written}
						`}
				</Code>	
				</div>
			</div>
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar>Weakest Precondition</titlebar>
		<mybody>
			<div>

				<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[60vw]">
					<Code lang="java" lines= "1-5"> 
						{`
					 {true}
					 action := writeThesis;
					 {action == writeThesis}
					 thesis := written; 
					 {action == writeThesis, thesis == written}
					 `}
				</Code>	
			</div>
			<br>
			<div>"true" is the most general (weakest) precondition.</div>
		</div>
		</mybody>
	</Layout></Slide>
	
	<script>newChapter("Weakest (Liberal) Precondition");</script>
	<Slide animate><Layout>
		<titlebar>
			Hoare Triple vs Weakest Precondition (Deterministic)
		</titlebar>
		<mybody>
			<Step fadeIn>
				<img src="hoare.svg" alt="wlp_d" class="w-[40vw] ">
			</Step>
			<Step fadeIn>
				<img src="wp-det.svg" alt="wlp_d" class="w-[40vw] ml-[5vw]">
			</Step>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			Weakest Precondition (Nondeterministic, Angelic)
		</titlebar>
		<mybody>
			<img src="wp-nondet.svg" alt="wlp_d" class="w-[50vw]">
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			Weakest Liberal Precondition (Nondeterministic, Demonic)
		</titlebar>
		<mybody>
			<img src="wlp-nondet.svg" alt="wlp_d" class="w-[50vw]">
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			wp, wlp, sp, slp
		</titlebar>
		<mybody>
			<Step fadeIn>
				<img src="wp-family.svg" alt="wlp_d" class="w-[48vw]">
			</Step>
			<Step fadeIn>
				<img src="sp-family.svg" alt="wlp_d" class="w-[47vw] ml-[2vw]">
			</Step>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			Proving Program Correct or Bugs Reachable
		</titlebar>
		<mybody>
			<div style="text-align: left;">
				<div data-id="total-correct">{`\$G\\implies wp.C.F\$`}.  Termination and correctness. (Manna and Pnueli, 1974)</div>
				<div data-id="partial-correct">{`\$G\\implies wlp.C.F\$`}. Correctness upon termination. (Hoare, 1969) </div>
				<!-- <br>Equivalent: {`\$sp.C.G\\implies F\$`} <br> -->
				<div data-id="total-incorrect">{`\$F\\implies sp.C.G\$`}. Bugs definitely reachable. (O'Hearn, 2020) </div>
				<div data-id="partial-incorrect">{`\$F\\implies slp.C.G\$`}. Bugs can be reachable. </div>
				<!-- <br>Equivalent: {`\$wp.C.F\\implies G\$`} -->
			</div>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			Proving Program Correct or Bugs Reachable
		</titlebar>
		<mybody>
			<div style="text-align: left;">
				<div data-id="total-correct">{`\$G\\implies wp.C.F\$`}.  Termination and correctness. (Manna and Pnueli, 1974)</div>
				<br>
				<div data-id="partial-correct">{`\$G\\implies wlp.C.F\$`}. Correctness upon termination. (Hoare, 1969) </div>
				<div class="ml-[5vw]">Equivalent: {`\$sp.C.G\\implies F\$`} </div>
				<br>
				<div data-id="total-incorrect">{`\$F\\implies sp.C.G\$`}. Bugs definitely reachable. (O'Hearn, 2020) </div>
				<br>
				<div data-id="partial-incorrect">{`\$F\\implies slp.C.G\$`}. Bugs can be reachable. </div>
				<div class="ml-[5vw]">Equivalent: {`\$wp.C.F\\implies G\$`}</div>
			</div>
		</mybody>
	</Layout></Slide>
	<Slide animate><Layout>
		<titlebar>
			How about the others?
		</titlebar>
		<mybody>
			<div style="text-align: left;" data-id="wlpg">
				<Step fadeOut> 
					{`\$slp.C.G\\implies F\$`}
				</Step>
				{`\$wlp.C.F\\implies G\$`}
			</div>
		</mybody>
	</Layout></Slide>
	
	<!-- Body -->
	<script>newChapter("Necessarily Liberal Precondition");</script>
	<Slide animate><Layout>
		<titlebar>
			Necessarily Liberal Precondition
		</titlebar>
		<mybody>
			<div style="text-align: left;" class="text-[10vh]" data-id="wlpg">
				{`\$wlp.C.F\\implies G\$`}
			</div>
		</mybody>
	</Layout></Slide>
	
	<Slide animate><Layout>
		<titlebar style="display:block; padding-top:0.7vh;">
			Remember: {`\$ wlp \$`}  transformer with demonic non-determinism 
		</titlebar>
		<mybody>
			<img src="wlpd.svg" alt="wlp_d" class="h-[50vh] ">
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar style="display:block; padding-top:0.7vh;">
			Investigating {`\$ G \$`} where {`\$ wlp.C.F\\implies G \$`}
		</titlebar>
		<mybody>
			<Stack> 
				<img src="wlpd.svg" alt="wlp_d" class="w-[40vw]"> 
				<Step><img src="g color.svg" alt="wlp_d" class="w-[40vw]"> </Step>
			</Stack>
			<Step> <img data-id="img:q" src="question with color.svg" alt="?" class="w-[40vw] ml-[10vw]"></Step>
		</mybody>
	</Layout></Slide>


	<Slide ><Layout>
		<titlebar style="display:block; padding-top:0.7vh;">
			{`\$ wlp.C.F\\implies G \$`}
		</titlebar>
		<mybody>
			<Stack>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
				<img data-id="wlp-g" src="wlp-g-g.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-gg.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-ggr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-ggrr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-r.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-rr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-gr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-grr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
				<Step fadeInThenOut> 
					<img data-id="wlp-g" src="wlp-g-ggrr.svg" alt="wlp-g" class="h-[50vh] flex items-center justify-center">
				</Step>
			</Stack>
		</mybody>
	</Layout></Slide>


	<Slide animate><Layout>
		<titlebar> Possibilities of G </titlebar>
		<mybody>
			<div class="grid" style="grid-template-columns: auto auto auto auto; row-gap:5vh;">
				<img  src="wlp-g.svg" alt="wlp-g">
				<img  src="wlp-g-g.svg" alt="wlp-g">
				<img data-id="wlp-gg" src="wlp-g-gg.svg" alt="wlp-g">
				<img  src="wlp-g-ggr.svg" alt="wlp-g">
				<img  src="wlp-g-ggrr.svg" alt="wlp-g">
				<img  src="wlp-g-r.svg" alt="wlp-g">
				<img  src="wlp-g-rr.svg" alt="wlp-g">
				<img  src="wlp-g-gr.svg" alt="wlp-g">
				<img  src="wlp-g-grr.svg" alt="wlp-g">
				<img  src="wlp-g-ggrr.svg" alt="wlp-g">
		</div>
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar> A Special G </titlebar>
		<mybody>
			<img data-id="wlp-gg" src="wlp-g-gg.svg" alt="wlp-g" class="w-[60vw]">
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar> A Special G </titlebar>
		<mybody>
			<ul class="a ml-[10vw]">
				<li> if {`\$ (wlp.C.F\\implies G)\ \\wedge\ (sp.C.\\neg G \\implies \\neg F) \$`} then
					{`\$ wlp_a.C.F \\implies G\$`}
				</li>
				<br>
				<li> if {`\$ (P{\\implies} G) \\implies \\neg(sp.C.P {\\implies} \\neg F) \$`} then 
					{`\$G {\\implies} wlp_a.C.F \$`}
				</li>
				<br>
			</ul>
		</mybody>
	</Layout></Slide>

	<Slide animate><Layout>
		<titlebar> The General G </titlebar>
		<mybody>
			<div class="grid" style="grid-template-columns: auto auto auto auto; row-gap:5vh;">
				<img  src="wlp-g.svg" alt="wlp-g">
				<img  src="wlp-g-g.svg" alt="wlp-g">
				<img data-id="wlp-gg" src="wlp-g-gg.svg" alt="wlp-g">
				<img  src="wlp-g-ggr.svg" alt="wlp-g">
				<img  src="wlp-g-ggrr.svg" alt="wlp-g">
				<img  src="wlp-g-r.svg" alt="wlp-g">
				<img  src="wlp-g-rr.svg" alt="wlp-g">
				<img  src="wlp-g-gr.svg" alt="wlp-g">
				<img  src="wlp-g-grr.svg" alt="wlp-g">
				<img  src="wlp-g-ggrr.svg" alt="wlp-g">
		</div>
		</mybody>
	</Layout></Slide>

	
	<Slide ><Layout>
		<titlebar> The General G </titlebar>
		<mybody>
			<div>
				<Step fadeIn>
					Conclusion: without further restrictions, {`\$ G \$`} can be anything. 
				</Step>
				<br>
				<Step fadeIn>
					<div class="text-[10vh]"> However: </div> 
				</Step>
			</div>
		</mybody>
	</Layout></Slide>

	<Slide ><Layout>
		<titlebar> The General G </titlebar>
		<mybody>
			<div>
				<div>
					<Step fadeIn>
						Lemma: {`\$ wlp.C.F = \\neg wp.C.\\neg F \$`} 
					</Step>
					<Step fadeIn>
						({`\$ wlp \$`} and {`\$ wp \$`} are each other's conjugate. )
					</Step>
				</div>
				<div class="flex justify-center items-center">
					<Stack> 
						<Step> 
							<img src="wlp-nwp.svg" alt="wlp-nwp" class="h-[50vh] flex items-center justify-center">
						</Step>
						<Step fadeIn> 
							<img src="wlp-nwp-fill.svg" alt="wlp-nwp" class="h-[50vh] flex items-center justify-center">
						</Step>
						<Step fadeIn> 
							<img src="g-fill.svg" alt="wlp-nwp" class="h-[50vh] flex items-center justify-center">
						</Step>
						<Step fadeIn> 
							<img src="ng-bracket.svg" alt="wlp-nwp" class="h-[50vh] flex items-center justify-center">
						</Step>
					</Stack>
				</div>
				<div>
					<Step>
						Conclusion: {`\$ \\neg G \\implies wp.C.\\neg F \$`}. 
					</Step>
				</div>
			</div>
		</mybody>
	</Layout></Slide>
	<Slide ><Layout>
		<titlebar> The General G </titlebar>
		<div>
			Conclusion: {`\$ \\neg G \\implies wp.C.\\neg F \$`}. 
		</div>
		<br>
		<Step fadeIn>
			"Negated total correctness"?
		</Step>
		<br>
		<Step fadeIn>
			{`\$ F \$`} can be used to express some unwanted behavior, with insufficient knowledge of more unwanted behavior: 
		</Step>
	</Layout></Slide>

	<script>newChapter("Example: Mutual Exclusion")</script>
	<Slide ><Layout>
		<titlebar> Example: Mutual Exclusion </titlebar>
		<div>
			Modifying Mutual Exclusion Protocol of Peterson: 
		</div>
		<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[70vw]">
			<Code lang="java" > 
				{`
					...
					// leave non-critical section
					turn := B;
					while (turn != A) do 
					    turn := A [] turn := B // modeling the behavior of other threads
					critA := true;
					// enter critical section
					... 
				`}
			</Code>	
		</div>
		<br>
		<div>
			Unwanted behavior: both A and B are in critical section. 
		</div>
		<div>
			{`\$ F = critA \\wedge critB \$`} 
		</div>
	</Layout></Slide>

	<Slide ><Layout>
		<titlebar> Example: Mutual Exclusion </titlebar>
		<div>
			Finding wlp: 
		</div>
		<br>
		<div class="mx-auto flex justify-center rounded items-center text-[7vh] bg-[#163468] w-[40vw]">
			<Code lang="java"  lines= "8|6-8|3-6|1-3"> 
				{`
						{ critB } 
					turn := B;
						{ critB }
					while (turn != A) do 
						turn := A [] turn := B 
						{ critB }
					critA := true;
						{ critA and critB }
				`}
			</Code>	
		</div>
	</Layout></Slide>

	<Slide ><Layout>
		<titlebar> Example: Mutual Exclusion </titlebar>
		<div>
			Thus by avoiding the satisfaction of critB before leaving non-critical section, thread A can be happy not to be deadlocked with thread B. 
		</div>
		<br>
		<Step fadeIn>
			<div>
				However, there might be multiple processes and threads that A does not know of. 
			</div>
		</Step>
		<br>
		<Step fadeIn>
			<div>
				The programmer might be aware of processes like kernel_task, systemstats, ... that might enter the same critical section: 
			</div>
		</Step>
		<br>
		<Step fadeIn>
			<div>
				{`\$ wlp.C.(critA \\wedge critB) = critB \$`} 
			</div>
			<div>
				{`\$ \\implies critB \\vee critKerneltask \\vee critSystemstats \$`}
			</div>
		</Step>
		<!-- <Step fadeIn>
			But we have insufficient knowledge of more potentially conflicting threads/processes, and can not formulate the condition "there is no deadlock" to use {`\$wp\$`}. 
		</Step>
	 -->
	</Layout></Slide>

	<!-- Conclusion -->
	<script> newChapter("Conclusion"); </script>
	<Slide><Layout>
		<titlebar> Conclusion </titlebar>

			<Step fadeIn>
				<div>
					With the help of {`\$sp,\\  wlp.C.F\\implies G \$`} can be used to identify {`\$ wlp\$`} with angelic non-determinism. 
				</div>
			</Step>
			<br>
			<Step fadeIn>
				Without extra specification, {`\$ G \$`} can contain anything. 
			</Step>
			<br>
			<Step fadeIn>
				It is however useful to avoid unwanted final states, when there is insufficient information about all unwanted behavior, but experienced programmer can overapproximate, or guess what final states are problematic. 
			</Step>
			<br>
			<Step fadeIn>
				In essence, it is useful instead of {`\$ wp \$`} when the postcondition is hard to express, but its negation is not. 
			</Step>
		</Layout>
	</Slide>
	<Slide><Layout>
		<titlebar> Reference </titlebar>
		<br><div> Thank you! </div>
	</Layout></Slide>

	<!-- to mimic the latex template  -->
	<script>
		const totalSlides = document.getElementsByTagName('pagenumber').length;
		const totalChapters = chapterNames.length;
		var temp; 
		for (let i = 0; i < totalSlides; i++) {
			// one section equals one slide, so just use section to locate slides
			document.getElementsByTagName('section')[i].setAttribute("id", "slide-"+(i+1));
			// add author name 
			document.getElementsByTagName('author')[i].innerHTML=author;
			// add title 
			document.getElementsByTagName('mytitle')[i].innerHTML="<a href=\"#slide-1\">"+title+"</a>";
			// show slide number with total slide number
			document.getElementsByTagName("pagenumber")[i].innerHTML = (i+1)+"/"+totalSlides;
		}
		for (let i = 0; i < totalSlides; i++) {
			for (let j = 0; j < totalChapters; j++) {
				// fill in chapter names
				temp = document.querySelectorAll("table.topbar")[i]; 
				temp.getElementsByTagName("tr")[0].innerHTML += "<th data-chpcol=\"chpcol\" style=\"font-weight:normal\">"+chapterNames[j]+"</th>";
				// document.getElementsByTagName("test")[0].innerHTML="here!"; 
				// fill in dots
				let slidesInChapter = presentationData.filter(item => item.chapter == chapterNames[j]);
				let begin = slidesInChapter[0].pagenr;
				let end = slidesInChapter[slidesInChapter.length-1].pagenr;
				let circles=toCircle(begin,end);
				temp.getElementsByTagName("tr")[1].innerHTML += "<td>"+ circles +"</td>";
			}
		}
		for (let i = 0; i < totalSlides; i++) {
			// first, find out if title page and toc exist 
			let chapter0 = presentationData.filter(item => item.chapternr == 0).length;
			if (i>=chapter0){
				// then set corresponding circle to white
				let circle = document.getElementsByTagName("circle-"+(i+1))[i]; 
				circle.style.background="white";
				circle.style.border="0px";
				// also the chapter names 
				let chapterNr = presentationData.filter(item => item.pagenr == i+1)[0].chapternr;
				temp = document.getElementsByTagName("th")[(i)*(totalChapters)+chapterNr-1];
				if (temp.hasAttribute("data-chpcol")){ 
					temp.style.color="white";
				}
			}
		}
		// circles: start, start+1, . . . , end are created
		function toCircle(begin,end){
			let circles = ""; 
			for (let i=begin; i<=end; i++){
				circles+="<a href=\"#slide-" +i+ "\"><circle-" +i+ " class=\"dot\"> </circle-" +i+ "> </a>" ;
			}
			return circles;
		}
		// to create table of contents
		if (toc) {
			for (i=1; i<=chapterNames.length;i++){
				document.getElementsByTagName('chpicons')[0].innerHTML += "<div class=\"chpicon\">"+i+"</div> <br>";
				document.getElementsByTagName('chpnames')[0].innerHTML += "<div>"+chapterNames[i-1]+"</div> <br>";
			}
		}
	</script>
	
	<style>
		:root{
			--g-size:30vh;
		}
		:global(.fragment.move) {
			height:70vh;
		}
		:global(.fragment.move.visible) {
			height:20vh;
		}
		:global(.fragment.blur) {
			filter: blur(5px);
		}

		:global(.fragment.blur.visible) {
			filter: none;
		}
		.chpicon{
			color: white;
			width:180%;
			background: var(--themecolor);
		}
		.dot {
			height: 15px;
			width: 15px;
			background: rgba(0,0,0,0);
			border-radius: 50%;
			border: 1.5px solid var(--themecolorlight);
			display: inline-block;
		}
		ul.a {
			list-style-type: square;
			padding-top:4vh;
		}
		ul.b {
			list-style-type: circle;
			margin-left:5vh;
			margin-right:5vh;
			padding-top:2vh;
		}
		ul{
			text-align: left;
			margin-left:20vh;
			margin-right:5vh;
		}
		titlebar{
			display:flex;
			background: rgb(242,242,242); 
			width: 100vw; 
			height: 9vh; 
			text-align: left;
			padding-left: 5vh; 
			padding-top: 2vh;
			font-size: 5vh;
		}
		mybody{
			display: flex;
			justify-content: center;
			align-items: center;
			height:79vh;
		}
	</style>
</Presentation>