# coursebuilder-course-homepage-dashboard
A layout change for the course homepage from a list to a dashboard approach which shows rough progress through each unit and powerful idea

##Modifications to the css (assets/css)

-Added gridism.css which allows boxes to be re-organised on collapsing and smaller windows.

-Added new styles to main.css for the course dashboard boxes, colours and hover links.

##Modifications to views/base_course.html

**Line #13** - `<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">`

Added `initial-scale=1` for use with gridism.css.

**Line #27** - `<link rel="stylesheet" href="assets/css/gridism.css">`

Added the gridism.css file reference so the dashboard can use it.

##Modifications to views/course.html

**Line #43 onwards**

Building the course homepage dashboard, dependant on when a particular unit is processed in the loop, can create a new line in the dashboard, include an image/badge or just insert the unit.
We do have some assessments that are used for help pages, or that have and are now disused. These are checked for inside the loop and nothing happens when they are discovered.

##Modifications to views/macros.html

**Line #34** - `macro render_heading_divs(title, desc)`

Displays the Powerful idea heading, for which are made up of a selection of units. Displayed above the units in its own row.

**Line #43** - `macro render_div_element(outline_element, percent_height, div_container_class, make_link=True)`

Displays the unit in the dashboard.

**Line #70** - `macro render_badge_div(height_percentage, badge_location, powerful_idea, div_container_class)`

Displays the image/badge associated with the powerful idea. This is displayed at the beginning of each row that has a selection of units that make up the releavnt powerful idea.

**Line #94** - `macro render_eocq_badge_div(height_percentage, badge_location, powerful_idea, div_container_class)`

Displays the End of course questionnaire image/badge in the dashboard.

##Modifications to the code at models/progress.py

###Additional methods added:

**Line #1002** - `get_unit_percent_complete_by_title(self, student)` added to class `UnitLessonCompletionTracker`

This method returns a dictionary with each unit's completion with the key being the unit title, and the value being a percentage represented by a float with the value between 0.0 (which is 0%) and 1.0 (which is 100%).
For example, the value represented by 0.45 is 45% in reality.

**Line #1045** - `get_powerful_idea_percent_complete(self, student, powerful_idea_units)` added to class `UnitLessonCompletionTracker`

Returns a percentage value of complete progress for the units that are specified by the parameter that is passed into the method, powerful_idea_units, for the student in question.

##Modifications to the code at modules/courses/lessons.py

###Modifications to method `get` in class `CourseHandler`

**Lines #167-201**

The code at these lines works out the percentage complete for each powerful idea, unit and if the unit is complete or not.
It puts those values through the calculate_fill_level method to get the matching fill level for the dashboard boxes based on each owns criteria.
It then adds those values to the template when it builds up the dashboard when called by the render method at the end of get method.

###Additional methods in class `CourseHandler`

**Line #243** - `calculate_fill_level`

This method calculates the fill level that should be applied to Unit and badge boxes on the dashboard, based on the criteria that the parameters match and returns the calculated fill level
