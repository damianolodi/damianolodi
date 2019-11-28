---
title: Data Visualization
linktitle: Matplotlib
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Data Analysis
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---


```py
import matplotlib.pyplot as plt
# or
from matplotlib import pyplot as plt
%matplotlib inline # display figures inside jupyter notebooks

print(plt.style.available)  # acess all available styles
plt.style.use('style-name') # set the style to be used

plt.plot(x_data, y_data,
         color='b',             # accept hex colors
         linestyle='-',         # or ls='-' || '--' '-.' ':' 'steps'
         marker='.',            # '+', 'o', '*', 's', ',', '.', '1', '2', '3',...
         linewidth=3,           # or lw=3
         markersize=2,          # or ms=2
         alpha=0.5,
         label='Legend label')


plt.xlabel('X label')   # label of x axis
plt.ylabel('Y label')   # label of y axis
plt.title('Title')      # title

plt.legend()            # make legend
plt.grid(True)          # make grid

plt.tight_layout()      # make better padding
plt.show()              # display the figure

plt.savefig('path.png', dpi=200) # save the plot
```

Lines are layered in the order they are added, so plot the bigger first and then add the smaller ones.

- `plt.xkcd()` &rarr; apply style to mimic _xkcd_ comics.
- `plt.gcf()` &rarr; stands for _get current figure._
- `plt.gca()` &rarr; stands for _get current axis._
- `plt.legend()` can have location attribute ([doc](https://matplotlib.org/tutorials/intermediate/legend_guide.html))

---
## Plotting Time Series Data

```py
...
import datetime as dt
from matplotlib import dates as mpl_dates

plt.plot_date(dates, y_data,    # dates in datetime format
              linestyle='solid')

plt.gcf().autofmt_xdate()       # rotates dates on the x axis to 45°

date_format = mpl_dates.DateFormatter('%b, %d %Y')  # set date format
plt.gca().xaxis.set_major_formatter(date_format)    # apply date format to datetime on x axis
```

[Datetime formatting codes](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior) (also used for _DateFormatter_).

---
## Subplots

```py
fig, ax = plt.subplots(
    nrows=2, ncols=1,            # create (nrows * ncols) axis inside the figure
    sharex=True, sharey=False,   # subplots can share axis
    figsize=(8,4), dpi=100)      # figsize in inch

plt.subplots_adjust(wspace=0, hspace=0) # set space between subplots

ax.plot(...)
ax.plot(...)

ax.set_xlabel('X label')
ax.set_ylabel('Y label')
ax.set_title('Title')

ax.set_xlim([min,max])  # set axis limits
ax.set_ylim([min,max])

ax.axis('log')          # set axis scale

ax.legend()

plt.tight_layout()

plt.show()

fig.savefig('path.png', dpi=200)
```

Each _ax_ (axis) is a plot, which can contain multiple lines. The _fig_ (figure) is the windows that contains all the axis created with subplots

If `nrows` and `ncols` are not defined in `subplots`, they _default to 1._

`ax` is a matrix with `nrows` rows and `ncols` columns. It can be unpacked to assign each axis to a single variable, e.g.

```py
fig, (ax1, ax2) = plt.subplots(nrows=2, ncols=1)
```

`legend` can have the following parameters

- `fontsize` &rarr; can be set to an _int_ or equal to _xx-small_, _x-small_, _small_, _medium_, _large_, _x-large_, _xx-large;_
- `frameon` &rarr; activate the frame around the legend and the background color.  


---
## Plot Annotation and Drawing

```py
# Annotate with text
ax.text(x, y,                   # coordinates
        'Hello world!',         # text
        family = 'monospace',
        fontsize = 10,
        rotation = 90           # text rotation
        )

# Draw line
ax.axvline(x = value,   # create vertical line in the plot
           color='r',
           linewidth=1)

# Draw arrow
ax.annotate(label,
      xy = (x, y),        # point position
      xytext = (x, y),    # text position
      arrowprops = dict(
          facecolor = 'black',
          headwidth = 4,
          width = 1,
          headlength = 4),
      horizontalalignment = 'left',
      verticalalignment = 'top')
```

---
## Plot Animations

1. `conda install -c conda-forge ffmpeg` &rarr; install `ffmpeg`.
- `import matplotlib.animation as animation` &rarr; import the animation package.
- `%matplotlib notebook` &rarr; allow dynamic plots in the Jupyter Notebook.
- Create the figure as usual.
- Create a writer with the following code.

```py
Writer = animation.writers['ffmpeg']
writer = Writer(
      fps = 20, # used when saving the movie in a file
      metadata = dict(artist = 'Damiano'),
      bitrate = 1800
      )
```

6. Create a function that will be used to animate the plot, like:

```py
def animate(i):
      data = data_prova.iloc[:int(i+1)]
      plot_ax.plot(data['Tempo Trascorso [s]'], data['P_tot '], 'b-', lw=1, ms=3)
      # plot a text on the figure and update it in each frame
      ## the probability_text variable needs to be initialized outside the function
      if data['P_tot '].iloc[int(i)]>=0.75:
        probability_text2.set_text('%.2f' % data['P_tot '].iloc[int(i)])
      else:
        probability_text.set_text('%.2f' % data['P_tot '].iloc[int(i)])
```

7. Create the animation object:

    -   `animate` is the name of the animation function. It can have more variables, but the `i` is incremented automatically;
    -   `interval` is the interval (in ms) between two frames in the animation;
    -   `frames` is the number of frames that are generated in that way.

```py
ani = matplotlib.animation.FuncAnimation(
                      plot_fig,
                      animate,
                      interval=int(frame_delay),
                      frames=len(data_prova['Tempo Trascorso [s]']),
                      repeat=False)
```

8. `ani.save(save_path+'/name.mp4', writer=writer)` &rarr; save.

---
## The `seaborn` Package

`sns.set()` set the style of the plot using `matplotlib` standards ([doc](http://seaborn.pydata.org/tutorial/aesthetics.html#aesthetics-tutorial))

---
## Custom Plot Style

_[Windows machine, Python installed using Miniconda 3 (for Anaconda it should be the same)]_

The idea is to create a **custom style file** in the same directory where all the styles are archived. The instruction reported on the documentation probably are correct only for a "normal" Python installation. This is the process to follow if Miniconda is installed:

1. navigate into the matplotlib style folder (mine is in `C:\Users\UserName\Miniconda3\Lib\site-packages\matplotlib\mpl-data\stylelib`).
- Create a file named `style-name.mplstyle` and write in it the styles as reported on the documentation.
- In the code use the following:

```python
import matplotlib.pyplot as plt
%matplotlib inline #if you are using jupyter
plt.style.use('style-name')
```

