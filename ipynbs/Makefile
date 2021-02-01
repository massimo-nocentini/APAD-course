
generators:
	jupyter-nbconvert generators.ipynb --to slides
	jupyter-nbconvert generators.ipynb --to rst
gotchas:
	jupyter-nbconvert gotchas.ipynb --to slides
	jupyter-nbconvert gotchas.ipynb --to rst
networkx:
	jupyter-nbconvert networkx.ipynb --to slides
	jupyter-nbconvert networkx.ipynb --to rst
matplotlib:
	jupyter-nbconvert matplotlib.ipynb --to slides
	jupyter-nbconvert matplotlib.ipynb --to rst
pandas:
	jupyter-nbconvert pandas.ipynb --to slides
	jupyter-nbconvert pandas.ipynb --to rst
numpy:
	jupyter-nbconvert numpy.ipynb --to slides
	jupyter-nbconvert numpy.ipynb --to rst
notebook:
	jupyter-nbconvert jupyter-notebooks.ipynb --to slides
	jupyter-nbconvert jupyter-notebooks.ipynb --to rst
introduction:
	jupyter-nbconvert introduction.ipynb --to slides
	jupyter-nbconvert introduction.ipynb --to rst
move:
	mv *.slides.html ../slides
	mv *.rst ../booklet/source

all: introduction notebook numpy pandas matplotlib networkx gotchas generators move
