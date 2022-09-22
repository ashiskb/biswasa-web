---
title: 'nbgrader configuration'
date: 2022-09-19
permalink: /posts/2022/09/biswas/blog-2-nbgrader/
tags:
  - nbgrader
  - autograding assignments
---

# Autograding with nbgrader
`nbgrader` is a tool that facilitates creating and `automatic` grading assignments in the Jupyter notebook. It allows instructors to easily create notebook-based assignments that include both coding exercises and written free-responses. `nbgrader` then also provides a streamlined interface for quickly grading completed assignments.

For an overview and demonstration of nbgrader’s core functionality, check out the talk on nbgrader given at SciPy 2017:
<iframe width="560" height="315" src="https://www.youtube.com/embed/5WUm0QuJdFw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Setting up nbgrader (for instructors)
1. Be sure to create a python virtual environment first.
2. Install with pip: `pip install nbgrader`
3. nbgrader extensions are supposed to be installed automatically with jupyter lab. But, let's enable them one by one:
```bash
jupyter labextension enable nbgrader/formgrader
jupyter labextension enable nbgrader/assignment-list
jupyter labextension enable nbgrader/course-list
jupyter labextension enable nbgrader/create-assignment
jupyter labextension enable nbgrader/validate-assignment
```
4. To install or enable the labextensions/server extension for just the current user, run the above commands with --user instead of --sys-prefix:
```bash
jupyter labextension enable --level=user nbgrader
jupyter server extension enable --user --py nbgrader
```
5. Also, run the following commands:
```bash
jupyter nbextension install --sys-prefix --py nbgrader --overwrite
jupyter nbextension enable --sys-prefix --py nbgrader
jupyter serverextension enable --sys-prefix --py nbgrader
```
6. To check the installed extensions, run: 
```bash
jupyter labextension list
jupyter server extension list
jupyter nbextension list
```
  You'll find the listed extensions :
  ```bash
  JupyterLab v3.4.7
/Users/ashis/venv-directory/venv-nbgrader/share/jupyter/labextensions
        jupyterlab_pygments v0.2.2 enabled OK (python, jupyterlab_pygments)
        nbgrader v0.8.0 enabled OK (python, nbgrader)
        @jupyter-widgets/jupyterlab-manager v5.0.3 enabled OK (python, jupyterlab_widgets)

  Config dir: /Users/ashis/.jupyter
    nbgrader enabled
    - Validating nbgrader...
      nbgrader 0.8.0 OK

Config dir: /Users/ashis/venv-directory/venv-nbgrader/etc/jupyter
    jupyterlab enabled
    - Validating jupyterlab...
      jupyterlab 3.4.7 OK
    nbclassic enabled
    - Validating nbclassic...
      nbclassic  OK
    nbgrader.server_extensions.formgrader enabled
    - Validating nbgrader.server_extensions.formgrader...
      nbgrader.server_extensions.formgrader  OK
    nbgrader.server_extensions.validate_assignment enabled
    - Validating nbgrader.server_extensions.validate_assignment...
      nbgrader.server_extensions.validate_assignment  OK
    nbgrader.server_extensions.assignment_list enabled
    - Validating nbgrader.server_extensions.assignment_list...
      nbgrader.server_extensions.assignment_list  OK
    nbgrader.server_extensions.course_list enabled
    - Validating nbgrader.server_extensions.course_list...
      nbgrader.server_extensions.course_list  OK
    notebook_shim enabled
    - Validating notebook_shim...
      notebook_shim  OK

Config dir: /usr/local/etc/jupyter
    nbgrader enabled
    - Validating nbgrader...
      nbgrader 0.8.0 OK

  
  Known nbextensions:
  config dir: /Users/ashis/.jupyter/nbconfig
    notebook section
      jupyter-js-widgets/extension  enabled 
      - Validating: OK
  config dir: /Users/ashis/venv-directory/venv-nbgrader/etc/jupyter/nbconfig
    notebook section
      nbextensions_configurator/config_menu/main  enabled 
      - Validating: problems found:
        - require?  X nbextensions_configurator/config_menu/main
      jupyter-js-widgets/extension  enabled 
      - Validating: OK
      create_assignment/main  enabled 
      - Validating: OK
      validate_assignment/main  enabled 
      - Validating: OK
    tree section
      nbextensions_configurator/tree_tab/main  enabled 
      - Validating: problems found:
        - require?  X nbextensions_configurator/tree_tab/main
      formgrader/main  enabled 
      - Validating: OK
      assignment_list/main  enabled 
      - Validating: OK
      course_list/main  enabled 
      - Validating: OK
  ```
7. To setup and run nbgrader quickly, you can create an example directory with example course files in it by running the `nbgrader quickstart` command:
```bash
nbgrader quickstart csci4930nbg
```
The above command will create a directory `csci4930nbg` with following directory tree, and a `gradebook.db` file. **Please move the `gradebook.db` file in the `csci4930nbg/` directory**:
  - Before moving the `gradebook.db`, execute command: `tree csci4930nbg`:
```bash
csci4930nbg
├── nbgrader_config.py
└── source
    ├── header.ipynb
    └── ps1
        ├── jupyter.png
        ├── problem1.ipynb
        └── problem2.ipynb

2 directories, 5 files
```
- After moving the `gradebook.db`, execute command: `tree csci4930nbg`:
```bash
csci4930nbg
├── nbgrader_config.py
├── gradebook.db
└── source
    ├── header.ipynb
    └── ps1
        ├── jupyter.png
        ├── problem1.ipynb
        └── problem2.ipynb

2 directories, 6 files
```
The `gradebook.db` is SQLite based database which you can open/manage with app like `DB Browser for SQLite` and you will find there are 14 tables created:
```bash
alembic_version
assignment
base_cell
comment
course
grade
grade_cells
notebook
solution-cells
source_cell
student
submitted_assignment
submitted_notebook
task_cells
```
You may find some dummy student entries in the `student` table.

8. **Adding students ids in database**. Let's write a text file `students.txt' containing a list of student ids:
```
ashiskb,Ashis Kumer,Biswas,ashiskb@yahoo.com
tri,Tri, Nguyen,tri@yahoo.com
edward,Edward,,ed@yahoo.com
```

Now, from a jupyter notebook execute the following:
```python
import pandas as pd
from nbgrader.api import Gradebook
# Create the connection to the database
with Gradebook('sqlite:///gradebook.db') as gb:
    file_path =  'students.txt'
    with open(file_path,"r") as f:
        for line in f:
            splits = line.split(",")
            id = splits[0]
            gb.add_student(id)
```
You can check the ids should be there in the database.student. Now from your notebook, click `Formgrader`, then click `Manage Students`. You can add first_name, last_name, etc from the `formgrader` with ease.
9. **Creating assignment**: In the `formgrader` in your jupyter notebook at your course directory, click `+ Add new assignment`. Then, name your assignment. Create a new notebook in it. Then, once the notebook is open, at the top in the toolbar click `View`-> `Cell Toolbar` -> `Create Assignment`.
  * By default each cell will have a dropdown menu with the “-” item selected. 
  * For markdown cells, there are three additional options to choose from, either “Manually graded answer”, “Manually graded task” or “Read-only”
  * For code cells, there are five options to choose from, including “Manually graded answer”, “Manually graded task”, “Autograded answer”, “Autograder tests”, and “Read-only”.
  * Here are the details of the cell types:
    * **Manually graded answer cells**: the nbgrader extension will mark that cell as a cell that contains an answer that must be manually graded by a human grader. The most common use case for this type of cell is for written free-response answers
    * **Manually graded task cells**: he nbgrader extension will mark that cell as a cell that contains the description of a task that students have to perform. They must be manually graded by a human grader. *The difference with a manually graded answer is that the manually graded tasks cells are not edited by the student.* A manually or automatically graded cell ask students to perform a task in one cell. A manually graded task asks students to perform a task with cells.
    * **Autograded answer** cells: the nbgrader extension will mark that cell as a cell that contains an answer which will be autograded.
      - Solutions in the answer cell can be specified inline, through the use of a special syntax such as `### BEGIN SOLUTION` and `### END SOLUTION`. 
      - When creating the release version (see Generate and release an assignment), the region between the special syntax lines will be replaced with a code stub, like the following. Note that the code stubs will be properly indented based on the indentation of the solution delimeters.
      ```python
      # YOUR CODE HERE
      raise NotImplementedError
      ```
      - If you would like to change the defaults, go edit the `nbgrader_config.py` file:
      ```python
      c = get_config()
      c.ClearSolutions.begin_solution_delimeter = "BEGIN MY SOLUTION"
      c.ClearSolutions.end_solution_delimeter = "END MY SOLUTION"
      c.ClearSolutions.code_stub = {
          "python": "# your code here\nraise NotImplementedError",
          "javascript": "// your code here\nthrow new Error();"
      }
      ```
      - Note that the code stub itself doesn’t have to cause an error (though that is the easiest thing to do, in my opinion) – it all depends on how you write your test cases. The only constraint is that when autograding happens, the behavior is such that:
        * If the tests pass, the student gets full credit.
        * If the tests fail, the student gets no credit.
        * So if the student hasn’t given an answer, the tests should ideally fail by default. How they fail is totally up to how you write your test cases.
      - Similarly, the text stub that the contents of Markdown cells get replaced with can be configured through the ClearSolutions.text_stub option:
      ```python
        c.ClearSolutions.text_stub = "Please replace this text with your response."
      ```
      - Unlike manually graded answers, autograded answers aren’t worth any points: instead, the points for autograded answers are specified for the particular tests that grade those answers.
    * **Autograder tests cells**:  the nbgrader extension will mark that cell as a cell that contains tests to be run during autograding.
      - Test cells should contain assert statements (or similar). 
      - When run through nbgrader autograde, the cell will pass if no errors are raised, and fail otherwise
      - You must specify the number of points that each test cell is worth; then, if the tests pass during autograding, students will receive the specified number of points, and otherwise will receive zero points.
      - Tests in “Autograder tests” cells can be hidden through the use of a special syntax such as `### BEGIN HIDDEN TESTS` and `### END HIDDEN TESTS`.
        * When creating the release version (see Generate and release an assignment), the region between the special syntax lines will be removed. 
        * Keep in mind that wrapping all tests (for an “Autograder tests” cell) in this special syntax will remove all these tests in the release version and the students will only see a blank cell. It is recommended to have at least one or more visible tests, or a comment in the cell for the students to see.
        * You can customize the default behavior of hidden tests by editing the `nbgrader_config.py` file:
        ```python
          c = get_config()
          c.ClearHiddenTests.begin_test_delimeter = "VERBORGE TOESTE BEGIN"
          c.ClearHiddenTests.end_test_delimeter = "VERBORGE TOESTE EINDIG"
        ``` 
    * **Read-only cells**: the nbgrader extension will mark that cell as one that cannot be modified. This is indicated by a lock icon on the left side of the cell toolbar. The read-only functionality will reverse any such changes made by the student.
10. **Validating assignments**: Ideally, the solutions in the instructor version should be correct and pass all the test cases to ensure that you are giving your students tests that they can actually pass. To verify this is the case, you can use the validate extension. 
  * If your assignment passes all the tests, you’ll get a success pop-up.
  * If it doesn’t pass all the tests, you’ll get a message telling you which cells failed.
  * You can validate an assignment from the commandline too:
  ```bash
  nbgrader validate source/ps1/*.ipynb
  ```
11. **Generate and release an assignment**: After an assignment has been created with the assignment toolbar, you will want to generate the version that students will receive. You can do this from the formgrader by clicking the “generate” button. 
  * This should succeed with a pop-up window containing log output.
  * You can generate an assignment from the command line too:

  ```bash
  nbgrader generate_assignment "ps1" --IncludeHeaderFooter.header=source/header.ipynb --force
  ```
   - Before you run the command above make sure to organize your assignment files in the source directory likes this:
   ```bash
    {course_directory}/source/{assignment_id}/{notebook_id}.ipynb
  ```
  - After running nbgrader generate_assignment, the release version of the notebooks will be:
  ```bash
{course_directory}/release/{assignment_id}/{notebook_id}.ipynb
  ```
  - As a reminder, the instructor is responsible for distributing this release version to their students using their institution’s existing student communication and document distribution infrastructure.
12. **Preview the student version**: After generating the student version of assignment, you should preview it to make sure that it looks correct. You can do this from the formgrader extension by clicking the “preview” button:
  * Under the hood, there will be a new folder called release with the same structure as source. The release folder contains the actual release version of the assignment files:
    ```bash
    release/ps1/problem1.ipynb
    release/ps1/problem2.ipynb
    ```
  * From the command line validate student release version with the following command:
    ```bash
    nbgrader validate --invert release/ps1/*.ipynb
    ```
    -  Ideally, all the tests should fail in the student version if the student hasn’t implemented anything. 
    - If the notebook fails all the test cases, you should see the message “Success! The notebook does not pass any tests.”
13. **Releasing files to students and collecting submissions**: At this point you will be able to take the files in the release folder and distribute them to students.
  - Similarly, to collect submissions, you can do this either with the formgrader extension or with the nbgrader collect command. Otherwise, you will need to manually place submitted files into the `submitted/` directory. You need to organize your files in a particular way. 
  - For submitted assignments, you should have the submitted versions of students’ assignments organized as follows:
  ```bash
submitted/{student_id}/{assignment_id}/{notebook_id}.ipynb
  ```
  - Please note: Students must use version 3 or greater of the IPython/Jupyter notebook for nbgrader to work properly. If they are not using version 3 or greater, it is possible for them to delete cells that contain important metadata for nbgrader. With version 3 or greater, there is a feature in the notebook that prevents cells from being deleted.
    - To ensure that students have a recent enough version of the notebook, you can include a cell such as the following in each notebook of the assignment:
    ```python
    import IPython
    assert IPython.version_info[0] >= 3, "Your version of IPython is too old, please update it."
    ```
14. **Autograding assignments**: There are two ways to autograde submissions:
  * From the `formgrader` extension: You can autograde individual submissions from the formgrader directly. To do so, click on the the number of submissions in the “Manage Assignments” view.
    * This will take you to a new page where you can see all the submissions.
    * For a particular submission, click the “autograde” button to autograde it.
    * After autograding completes, you will see a pop-up window with log output.
    * And back on the submissions screen, you will see that the status of the submission has changed to “needs manual grading” and there is now a reported score as well.
  * From commandline: 
    ```bash
    nbgrader autograde "ps1" --force
    ```
    - For example, when grading a particular submission  you may see some warnings that look like “Checksum for grade cell correct_squares has changed!”. What’s happening here is that nbgrader has recorded what the original contents of the grade cell correct_squares (when nbgrader generate_assignment was run), and is checking the submitted version against this original version. It has found that the submitted version changed (perhaps this student tried to cheat by commenting out the failing tests), and has therefore overwritten the submitted version of the tests with the original version of the tests.
    - You may also notice that there is a note saying “ps1 for studentx is 21503.948203 seconds late”. What is happening here is that nbgrader is detecting a file in Bitdiddle’s submission called timestamp.txt, reading in that timestamp, and saving it into the database.
    - From there, it can compare the timestamp to the duedate of the problem set, and compute whether the submission is at all late.
  * Once the autograding is complete, there will be new directories for the autograded versions of the submissions:
  ```bash
    autograded/{student_id}/{assignment_id}/{notebook_id}.ipynb
  ```
15. **Manual grading**: After assignments have been autograded, they will saved into an `autograded/` directory. 
  - We can manually grade assignments through the formgrader as well, by clicking on the “Manual Grading” navigation button. This will provide you with an interface for hand grading assignments that it finds in the directory listed above. Note that this applies to all assignments as well – as long as the autograder has been run on the assignment, it will be available for manual grading via the formgrader.
16. **Generate feedback on assignments**: after assignments have been autograded and/or manually graded, they will located in the autograded directory:
  ```bash
  autograded/{student_id}/{assignment_id}/{notebook_id}.ipynb
  ```
  * Generating feedback will create HTML files in the local instructor directory.
  * Releasing feedback will copy those HTML files to the nbgrader exchange.
  * We can generate feedback based on the graded notebooks by running the following command:
  ```bash
  nbgrader generate_feedback "ps1"
  ```
    - This will produce HTML versions of these notebooks at the following location:
    ```bash
    feedback/{student_id}/{assignment_id}/{notebook_id}.html
    ```
  * The `nbgrader generate_feedback` is also available by clicking the `Generate Feedback` button on either the `Manage Assignments` view (to generate feedback for all graded submissions), or on the individual student’s `Manage Submission` page (to generate feedback for that specific individual).
  * We can release the generated feedback by running the nbgrader `release_feedback command`, which will send the generated HTML files to the nbgrader exchange.
    - The `nbgrader release_feedback` is available by clicking the Release Feedback button on either the Manage Assignments view (to release feedback for all generated feedback), or on the individual student’s Manage Submission page (to release feedback for that specific individual).
17. **Getting grades from the database**: In addition to creating feedback for the students, you may need to upload grades to whatever learning management system your school uses (e.g. Canvas, Blackboard, etc.). nbgrader provides a way to export grades to CSV out of the box, with the `nbgrader export` command:
  ```bash
  nbgrader export
  ```
  * After running nbgrader export, you will see the grades in a CSV file called `grades.csv`:
  ```txt
  assignment,duedate,timestamp,student_id,last_name,first_name,email,raw_score,late_submission_penalty,score,max_score
  ps1,,[timestamp],bitdiddle,,,,1.5,0.0,1.5,13.0
  ps1,,[timestamp],hacker,,,,3.0,0.0,3.0,13.0
  ```
  * If you need to customize how the grades are exported, you can write your own exporter.
    
##References
* [Official nbgrader website](https://nbgrader.readthedocs.io/en/stable/index.html)
  - [Creating and grading assignments](https://nbgrader.readthedocs.io/en/stable/user_guide/creating_and_grading_assignments.html#manually-graded-answer-cells)