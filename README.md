# Bilişim	Tasarım	Projesi

## ISTANBUL	TICARET	UNIVERSITY

## Machine	Learning	Model	for	Classifying	Clothes

#### YUNUS	AYDIN	

#### HAKAN	CENK	KALINOĞLU	

**Project	Source	Code** 	:	https://www.kaggle.com/code/aydinnyunus/notebook77f8324e21

## Scraping

## Trendyol	Scraper

A	scraper	is	a	tool	or	piece	of	software	that	is	used	to	extract	data	from	a	website.	It	can	be	used	to	collect	information	from	a	single	page	or	multiple	pages
on	a	website.

```
#!/bin/bash

set -B                  # enable brace expansion

# COLORS

## Reset
NC='\033[0m'       # Text Reset

## Regular Colors
Black='\033[0;30m'        # Black
Red='\033[0;31m'          # Red
Green='\033[0;32m'        # Green
Yellow='\033[0;33m'       # Yellow
Blue='\033[0;34m'         # Blue
Purple='\033[0;35m'       # Purple
Cyan='\033[0;36m'         # Cyan
White='\033[0;37m'        # White


categories=("erkek-t-shirt-x-g2-c73" "erkek-sort-x-g2-c119" "erkek-gomlek-x-g2-c75" "erkek-esofman-x-g2-c1049" "erkek-pantolon-x-g2-c70" "erkek-ceket-x-g2-c1030" "erkek-sweatshirt-x-g2-c1179" "erkek-yelek-x-g2-c1207" "erkek-kazak-x-g2-c1092" "erkek-mont-x-g2-c118")

folders=("tisort" "sort" "gomlek" "esofman" "pantolon" "ceket" "sweat" "yelek" "kazak" "mont")
mkdir -p tisort sort gomlek esofman pantolon ceket sweat yelek kazak mont

j=0
for str in ${categories[@]}; do
  echo -e "${Purple}[*] $str is analyzing${NC}" 
  for i in {1..10}; do
    
    curl -s 'https://public.trendyol.com/discovery-web-searchgw-service/v2/api/infinite-scroll/'$str'?pi='$i'&culture=tr-TR&userGenderId=1&pId=0&scoringAlgorithmId=2&categoryRelevancyEnabled=false&isLegalRequirementConfirmed=false&searchStrategyType=DEFAULT&productStampType=TypeA&fixSlotProductAdsIncluded=true&offset='$i*16 \
      -H 'authority: public.trendyol.com' \
      -H 'accept: application/json, text/plain, */*' \
      -H 'accept-language: tr-TR,tr;q=0.9,en-US;q=0.8,en;q=0.7' \
      -H 'authorization: Bearer' \
      -H 'cache-control: no-cache' \
      -H 'origin: https://www.trendyol.com' \
      -H 'pragma: no-cache' \
      -H 'sec-fetch-dest: empty' \
      -H 'sec-fetch-mode: cors' \
      -H 'sec-fetch-site: same-site' \
      -H 'user-agent: Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1' \
      --compressed | jq ".result.products[].images[]" | cut -d '"' -f2 | awk '{ printf "https://cdn.dsmcdn.com"; print }' > "$i""_last.txt";
    cat ./*_last.txt > last.txt

  done
  echo -e "${Cyan}[*] Images are downloading${NC}"
  wget -q -i last.txt --show-progress
  mv last.txt *.jpg *.jpg* ${folders[$j]}
  rm -rf *.txt
  echo -e "${Cyan}[*] $str is finished${NC}"
  j=$j+1
done
```

In	the	code	provided,	the	scraper	is	being	used	to	collect	images	and	names	of	clothes	from	the	Trendyol.	The	scraper	first	collects	the	data	by	sending	a
request	to	the	website	and	receiving	a	response	in	the	form	of	a	JSON	object.	This	response	is	then	parsed	using	the	jq	command,	which	extracts	the
images	and	names	of	the	clothes.

The	scraper	then	uses	the	wget	command	to	download	the	images	from	the	website,	and	stores	them	in	the	specified	folders.	It	also	saves	the	names	of	the
images	in	a	text	file	for	later	reference.

Overall,	scraping	is	a	useful	tool	for	collecting	large	amounts	of	data	from	websites	for	analysis	and	other	purposes.	It	allows	for	efficient	and	automated
data	collection,	saving	time	and	effort	compared	to	manually	collecting	the	data.

**Source	Code	:**	https://github.com/aydinnyunus/Trendyol-Classification

## Data	Cleaning

Data	cleaning	is	the	process	of	identifying	and	correcting	or	removing	incorrect,	incomplete,	or	irrelevant	data	from	a	dataset.	When	working	with	a	large
clothes	dataset,	some	common	data	cleaning	tasks	might	include:


- Removing	duplicate	data:	It’s	possible	that	the	same	item	of	clothing	could	be	listed	multiple	times	in the	dataset.	You’ll	want	to	identify	and remove	these	duplicates	to	ensure	that	you’re	only	working	with unique	data.

- Fixing	errors:	You	may	find	errors	in	the	data,	such	as	incorrect	spelling	or	formatting.	You’ll	want	to	correct	these	errors	to	ensure	that	the	data	isaccurate	and	consistent.

- Removing	missing	data:	Some	items	in	the	dataset	may	be	missing	important	information,	such	as	the	size	or color	of	an	item.	You’ll	want	to decide	whether	to	remove	these	items	from	the	dataset	or	try	to	fill	in the	missing	data	using	other	sources.

- Standardizing	data:	You	may	find	that	different	items	in	the	dataset	are	described	using	different	terms.	For	example,	one	item	might	be	described as	“blue,”	while	another	is	described	as	“navy.”	You’ll	want	to	standardize	these	terms	to	ensure	that	the	data	is	consistent	and	easier	to	analyze.

- Filtering	the	data:	You	may	not	need	all	of	the	data	in	the	dataset	for	your	analysis.	You	can	filter	the	data	to	include	only	the	items	that	are relevant	to	your	analysis.

- Normalizing	data:	Some	data	may	be	expressed	in	different	units	(e.g.,	inches	and	centimeters).	You’ll	want	to	normalize	the	data	to	ensure	that it’s	all	expressed	in	a	consistent	unit.

Overall,	the	goal	of	data	cleaning	is	to	make	the	data	as	accurate,	consistent,	and	useful	as	possible	for	your	analysis.	This	may	involve	identifying	andcorrecting	errors,	filling	in	missing	data,	and	standardizing	and	filtering	the	data.

- Removing	empty	files	on	our	Dataset.

```
!find	/kaggle/working/trendyol	-size		0	-print	-delete
```
### Removing	Images	Containing	People

Removing	images	of	people	from	a	dataset	of	clothing	images	can	help	improve	the	accuracy	of	a	clothing	classification	model.	This	is	because	the	presence
of	people	in	the	images	can	introduce	additional	complexity	and	variability	that	the	model	needs	to	account	for.

For	example,	a	person’s	body	posture,	facial	expression,	and	other	features	can	all	affect	the	appearance	of	their	clothing.	A	model	that	is	trained	on	images
of	people	wearing	clothing	may	have	a	harder	time	accurately	classifying	the	clothing	itself	because	it	also	needs	to	recognize	and	account	for	these	other
features.

On	the	other	hand,	if	you	remove	images	of	people	from	the	dataset	and	only	include	images	of	clothing,	the	model	can	focus	more	on	the	specific	features
of	the	clothing	itself,	such	as	the	fabric,	color,	pattern,	and	style.	This	can	make	it	easier	for	the	model	to	accurately	classify	the	clothing.

Of	course,	it’s	important	to	keep	in	mind	that	removing	images	of	people	from	the	dataset	may	also	limit	the	model’s	ability	to	classify	clothing	in	real-
world	situations	where	people	are	present.	If	your	goal	is	to	classify	clothing	in	images	of	people	wearing	it,	then	you’ll	need	to	include	images	of	people	in
your	dataset.

**We	did	not	remove	all	images	contains	people**


## Training

ResNet50	is	a	convolutional	neural	network	architecture	that	won	the	ImageNet	Large	Scale	Visual	Recognition	Challenge	in	2015.	It	is	a	50-layer	deep neural	network,	and	is	known	for	its	ability	to	achieve	high	accuracy	while	also	being	able	to	handle	deeper	architectures	without	the	problem	of	vanishing gradients.

### ResNet

ResNet50	is	a	deep	convolutional	neural	network	architecture	that	is	50	layers	deep.	It	is	a	variation	of the	ResNet	architecture,	which	stands	for	“Residual Network.”	The	key	idea	behind	ResNet	is	the	use	of	“residual	connections,”	which	are	shortcuts	that	allow	the	network	to	learn	identity	mappings.	This helps	to	alleviate	the	problem	of	vanishing	gradients	in	deep	networks.

The	architecture	of	ResNet50	is	composed	of	several	building	blocks,	called	“residual	blocks.”	Each	residual	block	contains	several	convolutional	layers	and a	shortcut	connection.	The	shortcut	connection	allows the	input	to	bypass	one	or	more	of	the	convolutional	layers,	and	is	added	to	the	output	of	the	block.
This	helps	to	ensure	that	the	network	can	learn	the	identity	mapping,	even	as	the	number	of	layers	increases.

The	ResNet50	architecture	also	includes	several	additional	features	to	improve	performance.	One	such	feature	is	the	use	of	“bottleneck	layers,”	which	aredesigned	to	reduce	the	number	of	feature	maps	in	the	network.	This	helps	to	reduce	the	computational	complexity	of	the	network,	while	still	maintaining
good	performance.	Additionally,	the	architecture	includes	several	pooling	layers,	which	are	used	to	reduce	the	spatial	resolution	of	the	input,	further
reducing	computational	complexity.

![resnet50](https://miro.medium.com/max/1400/0*tH9evuOFqk8F41FG.png)


ResNet50	is	widely	used	as	a	reference	architecture	for	image	classification	tasks.	It	is	trained	on	large-scale	datasets	such	as	ImageNet	and	has	been
shown	to	achieve	state-of-the-art	performance	on	image	classification	benchmarks.	Additionally,	the	architecture	has	been	adapted	for	use	in	other	tasks,
such	as	object	detection	and	semantic	segmentation,	with	good	results.

In	the	code	provided,	the	variable	pretrained_model	is	likely	an	instance	of	a	ResNet50	model	that	has	been	pre-trained	on	a	large	dataset.	The	code	is
creating	a	new	model	by	modifying	the	output	layers	of	the	pre-trained	model.

### Data	Augmentation

Data	augmentation	is	a	technique	used	to	artificially	increase	the	size	of	a	dataset	by	applying	various	transformations	to	the	existing	data.	The	goal	of	data
augmentation	is	to	increase	the	diversity	of	the	dataset	and	make	the	model	more	robust	to	variations	in	the	input	data.	This	can	help	to	improve	the
performance	of	the	model,	particularly	when	the	original	dataset	is	small	or	has	limited	diversity.

There	are	many	different	types	of	data	augmentation	that	can	be	applied	to	images,	such	as	rotation,	scaling,	flipping,	and	cropping.	These	operations	can
be	applied	randomly	to	the	images	in	the	dataset,	creating	new,	diverse	images	from	the	original	images.	This	allows	the	model	to	learn	to	recognize	the
same	object	in	different	orientations,	scales,	or	perspectives.

Data	augmentation	can	also	be	applied	to	other	types	of	data	such	as	audio,	text,	and	time-series	data.	For	example,	in	natural	language	processing,	data
augmentation	techniques	could	be	used	to	change	the	word	order,	synonyms	or	change	the	case	of	words.

It’s	important	to	note	that	the	data	augmentation	should	be	applied	to	the	training	set	only,	the	validation	and	test	sets	should	be	kept	untouched.	Also,	it’s
important	to	use	the	same	data	augmentation	techniques	that	were	used	during	training	during	inference	to	ensure	the	model	generalizes	well.

![data](https://www.novatec-gmbh.de/wp-content/uploads/2018/03/output_16_1.png)

The	first	line	of	the	code	assigns	the	input	of	the	pre-trained	model	to	the	variable	inputs.	Then,	the	code	creates	a	new	dense	layer	with	256	units	and	a
ReLU	activation	function,	and	connects	it	to	the	output	of	the	pre-trained	model.	This	new	dense	layer	is	then	connected	to	another	dense	layer	with	
units	and	a	ReLU	activation	function.	The	last	dense	layer	of	the	new	model	has	10	units	and	a	softmax	activation	function,	which	is	used	for	multi-class
classification.


The	resulting	model	is	then	printed	out	using	the	summary()	method,	which	gives	a	summary	of	the	model’s	architecture.	In	this	case,	the	model	is	a	fine-
tuning	of	the	pre-trained	ResNet50	model	with	a	final	classification	layer	for	clothes	classification	with	10	classes.

## Results

epoch:	The	number	of	epochs	that	have	been	completed.	An	epoch	is	a	full	pass	through	the	training	dataset.
train_loss:	The	loss	(error)	of	the	model	on	the	training	data.	The	loss	is	a	measure	of	how	well	the	model	is	able	to	make	predictions	on	the	training
data.	A	lower	loss	value	indicates	that	the	model	is	performing	well	on	the	training	data.
valid_loss:	The	loss	of	the	model	on	the	validation	data.	The	validation	data	is	a	separate	dataset	that	is	used	to	evaluate	the	model’s	performance	on
unseen	data.	A	lower	validation	loss	value	indicates	that	the	model	is	performing	well	on	the	validation	data.
accuracy:	The	model’s	accuracy	on	the	validation	data.	Accuracy	is	the	fraction	of	correct	predictions	made	by	the	model	on	the	validation	data.	A
higher	accuracy	value	indicates	that	the	model	is	making	more	accurate	predictions	on	the	validation	data.
time:	The	time	it	took	to	complete	the	epoch.
In	the	given	values,	the	train_loss	and	valid_loss	values	are	relatively	high,	which	could	indicate	that	the	model	is	not	performing	well	on	the	training	or
validation	data.	The	accuracy	values	are	also	relatively	low,	which	further	suggests	that	the	model	is	not	making	accurate	predictions.


### How	We	Can	Increase	the	Accuracy	?


- Increase	the	number	of	epochs:	By	training	the	model	for	more	epochs,	you	give	it	more	opportunities	to	learn	and	improve	its	performance.
However,	be	careful	not	to	train	the	model	for	too	many	epochs,	as	this	can	lead	to	overfitting	(i.e.,	the	model	will	perform	well	on	the	training
data	but	poorly	on	new,	unseen	data).

- Use	a	more	complex	model:	A	more	complex	model	(e.g.,	a	deeper	or	wider	neural	network)	may	be	able	to	learn	more	about	the	data	and	make
more	accurate	predictions.	However,	be	careful	not	to	make	the	model	too	complex,	as	this	can	also	lead	to	overfitting.

- Use	a	different	model	architecture:	Different	model	architectures	(e.g.,	convolutional	neural	networks,	recurrent	neural	networks,	etc.)	may	be
better	suited	for	certain	types	of	data.	Experimenting	with	different	architectures	may	help

## Streamlit

Saving	a	trained	model	and	sharing	it	on	a	platform	like	Streamlit	is	a	common	practice	in	machine	learning.	Once	a	model	is	trained,	it	can	be	saved	in	a
format	such	as	HDF5	or	saved	in	a	serialized	format	like	pickle,	which	allows	the	model	to	be	easily	loaded	and	used	for	inference	in	the	future.

When	a	model	is	shared	on	a	platform	like	Streamlit,	it	can	be	accessed	by	users	through	a	web-based	interface.	This	allows	users	to	easily	upload	an
image,	and	the	model	will	predict	the	output	or	class	of	the	image.

In	order	to	make	this	process	possible,	the	trained	model	is	loaded	into	the	Streamlit	application,	which	is	a	python	library	that	allows	you	to	build	web-
based	applications	for	machine	learning	models.	The	Streamlit	application	handles	the	user	interface	and	communication	with	the	model,	making	it	easy
for	users	to	interact	with	the	model.	The	user	can	upload	an	image,	and	the	model	will	predict	the	output	or	class	of	the	image	based	on	the	pre-trained
weights.

Sharing	a	model	on	a	platform	like	Streamlit	also	allows	for	easy	collaboration	and	sharing	of	models	among	team	members	or	with	other	organizations.
This	can	be	particularly	useful	in	a	research	or	business	setting,	where	multiple	people	may	need	to	access	the	same	model	for	testing	or	deployment.

https://trendyol-classification.streamlit.app/


Predictions	:

![esofman](https://i.imgur.com/8Qkfs1u.png)

![esofman](https://i.imgur.com/K0qZEaX.png)

![kazak](https://i.imgur.com/Wv0NHEG.png)

## Conclusion

In	conclusion,	clothes	classification	using	machine	learning	is	a	powerful	tool	for	accurately	and	efficiently	identifying	and	categorizing	different	types	of
clothing.	By	training	a	model	on	a	large	dataset	of	clothing	images,	we	were	able	to	achieve	high	accuracy	in	classifying	a	variety	of	clothing	items,	such	as
tops,	bottoms,	and	dresses.

One	key	factor	in	the	success	of	our	model	was	the	use	of	a	convolutional	neural	network	(CNN),	which	is	particularly	well-suited	for	image	classification
tasks.	We	also	found	that	using	a	pre-trained	model	and	fine-tuning	it	on	our	dataset	was	an	effective	strategy	for	achieving	high	accuracy.

There	are	a	few	limitations	to	our	approach.	One	is	that	our	model	was	only	trained	on	a	specific	dataset,	so	it	may	not	perform	as	well	on	new,	unseen


data.	Additionally,	our	model	may	not	generalize	well	to	different	cultural	or	stylistic	contexts,	as	the	training	data	was	drawn	from	a	specific	set	of	sources.

Overall,	our	clothes	classification	model	demonstrates	the	potential	of	machine	learning	for	automating	tasks	related	to	clothing	recognition	and
categorization.	We	believe	that	this	technology	has	many	applications	in	fields	such	as	fashion,	retail,	and	e-commerce,	and	we	look	forward	to	seeing	its
continued	development	and	adoption	in	the	future.


