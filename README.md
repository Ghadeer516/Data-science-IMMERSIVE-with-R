# Data-science-IMMERSIVE-with-R
#3.2.4 Exercises
mpg
df<- mpg
#1 Run ggplot(data = mpg). What do you see?
ggplot(data = mpg)
#nothing
#2-How many rows are in mpg? How many columns?
# A data frame with 234 rows and 11 variables

#3-What does the drv variable describe? Read the help for ?mpg to find out.
#?mpg
#the type of drive train, where f = front-wheel drive, r = rear wheel drive, 4 = 4wd

#4-Make a scatterplot of hwy vs cyl.
ggplot(mpg, aes(x = cyl, y = hwy)) +
  geom_point()

#5-What happens if you make a scatterplot of class vs drv? Why is the plot not useful?
ggplot(mpg, aes(x = class, y = drv)) +
  geom_point()
# variable class is categorical not working

#3.3.1 Exercises
#1-What’s gone wrong with this code? Why are the points not blue?
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy, color = "blue"))
#The argumentcolour = "blue" is included within the mapping argument, and as such, it is treated as an aestheti
#, which is a mapping between a variable and a value. In the expression,
#colour = "blue", "blue" is interpreted as a categorical variable which only takes a single value "blue".
#If this is confusing, consider how colour = 1:234 and colour = 1 are interpreted by aes().
#The following code does produces the expected result.
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy), colour = "blue")
#2-which variables in mpg are categorical? Which variables are continuous?
#(Hint: type ?mpg to read the documentation for the dataset).How can you see this information when you run mpg?
glimpse(mpg)
#3-Map a continuous variable to color, size, and shape. How do these aesthetics behave differently for categorical vs.
#continuous variables?
#The variable cty, city highway miles per gallon, is a continuous variable.Using color:
ggplot(mpg, aes(x = displ, y = hwy, colour = cty)) +
  geom_point()
#usung size:
ggplot(mpg, aes(x = displ, y = hwy, size = cty)) +
  geom_point()
#Using shape:
ggplot(mpg, aes(x = displ, y = hwy, shape = cty)) +
  geom_point()
#> Error: A continuous variable can not be mapped to shape
#A numeric variable has an order, but shapes do not. 
#It is clear that smaller points correspond to smaller values, or once the color scale is given, 
#which colors correspond to larger or smaller values. But it is not clear whether a square is greater or less than a circle.

#4-What happens if you map the same variable to multiple aesthetics?
ggplot(mpg, aes(x = displ, y = hwy, colour = hwy, size = displ)) +
  geom_point()
# Mapping a single variable to multiple aesthetics is redundant.
#Because it is redundant information, in most cases avoid mapping a single variable to multiple aesthetics.
#5-What does the stroke aesthetic do? What shapes does it work with? (Hint: use ?geom_point)
?geom_point
#Stroke changes the size of the border for shapes (21-25)
#6-What happens if you map an aesthetic to something other than a variable name,
#like aes(colour = displ < 5)? Note, you’ll also need to specify x and y.
#for example
ggplot(mpg, aes(x = displ, y = hwy, colour = displ < 5)) +
  geom_point()
#it will show the result of displ < 5 in a logical variable which takes values of TRUE or FALSE.
#3.5.1 Exercises
#1-What happens if you facet on a continuous variable?
#The continuous variable is converted to a categorical variable, and the plot contains a facet for each distinct value.
#2-What do the empty cells in plot with facet_grid(drv ~ cyl) mean? How do they relate to this plot?
ggplot(data = mpg) +
  geom_point(mapping = aes(x = hwy, y = cty)) +
  facet_grid(drv ~ cyl)
#The empty cells (facets) in this plot are combinations of drv and cyl that have no observations.
#These are the same locations in the scatter plot of drv and cyl that have no points.
ggplot(data = mpg) +
  geom_point(mapping = aes(x = drv, y = cyl))
#3-What plots does the following code make? What does . do?
# ignores that dimension when faceting.
  ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(drv ~ .)
#4-Take the first faceted plot in this section:
  ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy)) + 
  facet_wrap(~ class, nrow = 2)
#-What are the advantages to using faceting instead of the colour aesthetic? What are the disadvantages? How might the balance change if you had a larger dataset?
    #Advantages of encoding class with facets instead of color include the ability to encode more distinct categories.
     #and disadvantages the difficulty of comparing the values of observations between categories since the observations for each category are on different plots.
#5-Read ?facet_wrap. What does nrow do? What does ncol do? What other options control the layout of the individual panels? Why doesn’t facet_grid() have nrow and ncol arguments?
    #When using facet_grid() you should usually put the variable with more unique levels in the columns. Why?
    #The arguments nrow (ncol) determines the number of rows (columns) to use when laying out the facets. It is necessary since facet_wrap() only facets on one variable.
    #The nrow and ncol arguments are unnecessary for facet_grid() since the number of unique values of the variables specified in the function determines the number of rows and columns.
#3.6.1 Exercises:
#1-What geom would you use to draw a line chart? A boxplot? A histogram? An area chart?
  ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
    geom_point() + 
    geom_smooth(se = FALSE)
  #line chart: geom_line()
  #boxplot: geom_boxplot()
  #histogram: geom_histogram()
  #area chart: geom_area()
#2-Run this code in your head and predict what the output will look like. Then, run the code in R and check your predictions.
  ggplot(data = mpg, mapping = aes(x = displ, y = hwy, color = drv)) + 
    geom_point() + 
    geom_smooth(se = FALSE)
    #This code produces a scatter plot with displ on the x-axis, hwy on the y-axis, and the points colored by drv.
    #There will be a smooth line, without standard errors, fit through each drv group.
#3-What does show.legend = FALSE do? What happens if you remove it?Why do you think I used it earlier in the chapter?
    # show.legend = FALSE hides the legend box.
#4-What does the se argument to geom_smooth() do?
    #It adds standard error bands to the lines
  ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
    geom_point() +
    geom_smooth(se=TRUE)
  #without
  ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
    geom_point() +
    geom_smooth(se=FALSE)
#5-Will these two graphs look different? Why/why not?
  ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
    geom_point() +
    geom_smooth()
  
  ggplot() +
    geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) +
    geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
    #No. Because both geom_point() and geom_smooth() will use the same data and mappings
#6-Recreate the R code necessary to generate the following graphs.
  #First1:
  ggplot(mpg, aes(x = displ, y = hwy)) +
    geom_point() +
    geom_smooth(se = FALSE)
  #second:
  ggplot(mpg, aes(x = displ, y = hwy)) +
    geom_smooth(mapping = aes(group = drv), se = FALSE) +
    geom_point()
  #third:
  ggplot(mpg, aes(x = displ, y = hwy, colour = drv)) +
    geom_point() +
    geom_smooth(se = FALSE)
  #fourth:
  ggplot(mpg, aes(x = displ, y = hwy)) +
    geom_point(aes(colour = drv)) +
    geom_smooth(se = FALSE)
  #fifth:
  ggplot(mpg, aes(x = displ, y = hwy)) +
    geom_point(aes(colour = drv)) +
    geom_smooth(aes(linetype = drv), se = FALSE)
  #six:
  ggplot(mpg, aes(x = displ, y = hwy)) +
    geom_point(size = 4, color = "white") +
    geom_point(aes(colour = drv))
#3.7.1 Exercises
#1-What is the default geom associated with stat_summary()? 
#How could you rewrite the previous plot to use that geom function instead of the stat function?
  ggplot(data = diamonds) +
    geom_pointrange(
      mapping = aes(x = cut, y = depth),
      stat = "summary",
      fun.min = min,
      fun.max = max,
      fun = median
    )
#2-What does geom_col() do? How is it different to geom_bar()?
    #The geom_col() function has different default stat than geom_bar().
  # The default stat of geom_col() is stat_identity(), which leaves the data as is.
  # The geom_col() function expects that the data contains x values and y values which represent the bar height.
      #The default stat of geom_bar() is stat_count(). The geom_bar() function only expects an x variable. The stat,
  #stat_count(), preprocesses input data by counting the number of observations for each value of x.
  #The y aesthetic uses the values of these counts.
#3-Most geoms and stats come in pairs that are almost always used in concert.
#Read through the documentation and make a list of all the pairs. What do they have in common?
    #These pairs of geoms and stats tend to have their names in common, such stat_smooth() and geom_smooth() and be documented on the same help page.
  # The pairs of geoms and stats that are used in concert often have each other as the default stat (for a geom) or geom (for a stat).
#4-What variables does stat_smooth() compute? What parameters control its behaviour?
    #The function stat_smooth() calculates the following variables:
  #y: predicted value
  #ymin: lower value of the confidence interval
  #ymax: upper value of the confidence interval
  #se: standard error
  #The parameters that control the behavior of stat_smooth() include:method-formula-method.arg()-se-na.rm.
#5-In our proportion bar chart, we need to set group = 1. Why?
#In other words what is the problem with these two graphs?
    ggplot(data = diamonds) + 
    geom_bar(mapping = aes(x = cut, y = after_stat(prop)))
  ggplot(data = diamonds) + 
    geom_bar(mapping = aes(x = cut, fill = color, y = after_stat(prop)))
  #If group = 1 is not included, then all the bars in the plot will have the same height, a height of 1.
  # The function geom_bar() assumes that the groups are equal to the x values since the stat computes the counts within the group.
  #The problem with these two plots is that the proportions are calculated within the groups.

    
#3.8.1 Exercises
#1-What is the problem with this plot? How could you improve it?
  ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) + 
    geom_point()
  #There is overplotting because there are multiple observations for each combination of cty and hwy values.
  # I would improve the plot by using a jitter position adjustment to decrease overplotting.
  ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
    geom_point(position = "jitter")
 #2-hat parameters to geom_jitter() control the amount of jittering?
  #there are two arguments to jitter:
  #width controls the amount of horizontal displacement, and
  #height controls the amount of vertical displacement.
#3-Compare and contrast geom_jitter() with geom_count().
  #there is no universal solution to overplotting. 
  #The costs and benefits of different approaches will depend on the structure of the data and the goal of the data scientist.
  
#4-What’s the default position adjustment for geom_boxplot()? Create a visualisation of the mpg dataset that demonstrates it. 
  #When we add colour = class to the box plot, the different levels of the drv variable are placed side by side, i.e., dodged.
  ggplot(data = mpg, aes(x = drv, y = hwy, colour = class)) +
    geom_boxplot()
  
  #If position_identity() is used the boxplots overlap.
  ggplot(data = mpg, aes(x = drv, y = hwy, colour = class)) +
    geom_boxplot(position = "identity")
