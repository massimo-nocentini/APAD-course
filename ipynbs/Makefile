
exercises:
	#jupyter-nbconvert generators.ipynb --to slides
	jupyter-nbconvert exercises.ipynb --to rst
	#echo "Generators\n##########\n\n" | cat - generators.rst > temp && mv temp generators.rst
core-concepts:
	#jupyter-nbconvert generators.ipynb --to slides
	jupyter-nbconvert core-concepts.ipynb --to rst
	#echo "Generators\n##########\n\n" | cat - generators.rst > temp && mv temp generators.rst
gotchas:
	jupyter-nbconvert gotchas.ipynb --to slides
	jupyter-nbconvert gotchas.ipynb --to rst
	echo "Tips an' Tricks\n###############\n\n" | cat - gotchas.rst > temp && mv temp gotchas.rst
networkx:
	jupyter-nbconvert networkx.ipynb --to slides
	jupyter-nbconvert networkx.ipynb --to rst
	echo "``networkx`` module\n###################\n\n" | cat - networkx.rst > temp && mv temp networkx.rst
matplotlib:
	jupyter-nbconvert matplotlib.ipynb --to slides
	jupyter-nbconvert matplotlib.ipynb --to rst
	echo "``matplotlib`` module\n#####################\n\n" | cat - matplotlib.rst > temp && mv temp matplotlib.rst
pandas:
	jupyter-nbconvert pandas.ipynb --to slides
	jupyter-nbconvert pandas.ipynb --to rst
	echo "``pandas`` module\n#################\n\n" | cat - pandas.rst > temp && mv temp pandas.rst
numpy:
	jupyter-nbconvert numpy.ipynb --to slides
	jupyter-nbconvert numpy.ipynb --to rst
	echo "``numpy`` module\n################\n\n" | cat - numpy.rst > temp && mv temp numpy.rst
notebook:
	jupyter-nbconvert jupyter-notebooks.ipynb --to slides
	jupyter-nbconvert jupyter-notebooks.ipynb --to rst
	echo "Jupyter notebooks\n#################\n\n" | cat - jupyter-notebooks.rst > temp && mv temp jupyter-notebooks.rst
introduction:
	jupyter-nbconvert introduction.ipynb --to slides
	jupyter-nbconvert introduction.ipynb --to rst
	echo "Introduction\n############\n\n" | cat - introduction.rst > temp && mv temp introduction.rst
move:
	#mv *.slides.html ../slides
	mv *.rst ../booklet/source
	rm -rf ../booklet/source/*_files
	mv -f *_files ../booklet/source
	cp -r images ../booklet/source
	cp -r images ../booklet/source/_static
	cp *.slides.html ../booklet/source/_static

all: introduction notebook numpy pandas matplotlib networkx gotchas core-concepts exercises move
