---
layout: post
title:  "How to use Python as an astronomer"
date:   2019-03-15 21:40:00
author: Jiaxuan Li
categories: Coding
---

# Data Visualization

### [Book: Fundamentals of Data Visualization](https://serialmentor.com/dataviz/visualizing-amounts.html)

Really really nice book on principles of plotting and visualizing data. Like it! I will post some content below later.

### [My own notes for plotting in python](https://nbviewer.jupyter.org/github/astrojacobli/astro-ph/blob/master/Notes%20for%20Coding.ipynb)
### [Creating Lupton RGB images](http://docs.astropy.org/en/stable/visualization/lupton_rgb.html)

### Other packages for plot

- [mpld3](http://mpld3.github.io/notebooks/interactive_legend.html): renderer interactive figures (using d3) for Matplotlib code. Quite nice, though incomplete. Worth trying!
- [Altair](https://github.com/altair-viz/altair) is a relatively new declarative visualization library for Python. It's easy to use and makes great looking plots, however the ability to customize those plots is not nearly as powerful as in Matplotlib.
- [plot.ly](https://plot.ly/) can generate nice plots - this used to be a paid service only but was recently open sourced. Looks fancy!

# Useful Python packages in astrophysics:

- [`healpy`](https://healpy.readthedocs.io/en/latest/install.html): It is a package dealing with data on a sphere. It can map every direction to a pixel position, and vice versa. It can also calculate power spectrum of things like CMB. Although it's super useful, its python documentation is really unfriendly for beginners.

- [`starry`](https://rodluger.github.io/starry/tutorials/hd189.html): very cool package that can calculate light curves of transits and secondary eclipses of exoplanets, light curves of eclipsing binaries, rotational phase curves of exoplanets, light curves of planet-planet and planet-moon occultations, and more. A very cool tutorial: [A map of the hot jupiter HD 189733b](https://rodluger.github.io/starry/tutorials/hd189.html).

- [`cosmology`](https://github.com/esheldon/cosmology): cosmology package based on C, written by esheldon. Fast calculate distances. It can also be replaced by [`astropy.cosmology`](http://docs.astropy.org/en/stable/cosmology/index.html#module-astropy.cosmology).

- [`corner`](https://corner.readthedocs.io/en/latest/): A package of drawing corner diagrams of MCMC and 2d histogram. It's very easy to use and has many options. Good tool! You can find some examples in the `SN cosmology.ipynb`. [Song Huang](http://dr-guangtou.github.io) also uses `corner` to "explore" data: plot everything with everything. It can check the dependence between variables directly. Example by Song Huang: 

```python
import corner 
sdss_labels = [
      r'$u-g$', r'$g-i$',
      r'$\log \mathrm{EW}_{\mathrm{H}\alpha}$',
      r'$\log \mathrm{EW}_{\mathrm{H}\beta}$',
      r'$\log \mathrm{EW}_{[\mathrm{OIII}]}$'
  ]

sdss_corner = corner.corner(
      sdss_obs, bins=15, color=plt.get_cmap('coolwarm')(0.9),
      smooth=0.5, labels=sdss_labels,
      label_kwargs={'fontsize': 22},
      quantiles=[0.16, 0.5, 0.84],
      plot_contours=True, fill_contours=True,
      show_titles=True, title_kwargs={"fontsize": 18},
      hist_kwargs={"histtype": 'stepfilled', "alpha": 0.8, "edgecolor": "none"},
      use_math_text=True)

sdss_corner.set_size_inches(14, 14)
```

- [`multiprocess.Pool`](https://docs.python.org/3.7/library/multiprocessing.html): simple tool to run your loop tasks on multiple cores. This is embedded in `python` itself, and goes like below. For more complicated usage (multiple arguments as input), please refer to the documentation.

    - ```python
      from multiprocessing import Pool
      def run_SBP(obj):
          slug.run_SBP(obj)

      print('Number of processor to use:')
      n_jobs = int(input())

      pool = Pool(n_jobs)
      pool.map(run_SBP, obj_cat)
      ```

- [`joblib`](https://joblib.readthedocs.io/en/latest/parallel.html): an awesome tool of building pipelines in python. Just like a superior `pickle`, it can dump and load functions and data (especially large data arrays) efficiently. It can also distribute loop tasks to multiple processors. See the usages below:

  - ```python
    # Parallel computing
    from joblib import Parallel, delayed
    def run_SBP(obj):
        slug.run_SBP(obj)
        
    n_jobs = 4
    with Parallel(n_jobs=n_jobs, backend='loky') as parallel:
        parallel(delayed(run_SBP)(obj) for obj in obj_cat)
    ```

    There are various backends for this `Parallel` function, such as `loky`, `threading` and also `multiprocessing` as stated above. 

    

  - ```python
    # Dump and load
    from sklearn.neighbors import KNeighborsClassifier
    knn = KNeighborsClassifier()
    knn.fit(X_train, y_train)
    print(metrics.classification_report(y_test, knn.predict(X_test)))
    
    # After training a classifier using scikit-learn, 
    # you want to dump it and use it next time.
    import joblib
    joblib.dump(knn, './clf_cache/knn.joblib')
    # If you want to dump the file after compressing, use this.
    joblib.dump(knn, './clf_cache/knn.joblib.compress', compress=True)  
    ```

- [`ipyparallel`](https://ipyparallel.readthedocs.io/en/latest/index.html): Using IPython for parallel computing



# Tricks

### Jupyter Notebook

- `%%time`: Magic command to time your cell for a single run. Useful!
- `%%timeit`: this command can be used to test your code's performance. It runs your cell for many times and calculate the mean time consumed. 
- `%matplotlib inline`: show Matplotlib figures inline. Useful!

- `%run ./two-histograms.ipynb`: this will run the notebook directly and shows every output from that notebook under this cell.
- `%load ./download_tri_color_images.py`: load the functions and constants defined in the input python file, and run this file.
- `%config InlineBackend.figure_format ='retina'`: show high resolution figures for Mac Retina screen. This won't affect the figure which is saved, it only affect the figure which is showed using `plt.show()`.
- `_` means the previous output. By adding `;` in the end of a command, it will suppress the useless output of a function.
  - Basic shell commands: `!` is external shell command, and `&` is to run this command as background.

- Click + `control`: Jupyter supports multiple cursors, similar to Sublime Text. (Use `alt` on Windows)
- [Jupyter extensions](https://github.com/ipython-contrib/jupyter_contrib_nbextensions): my favorite extensions are  Codefolding, Nofity and Code prettify.