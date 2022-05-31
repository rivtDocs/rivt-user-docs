Welcome to rivt user documentation
===================================

    rivt is open source software for systematically writing engineering
    calculations. The calculation input file is written in rivtText which is
    designed to be easily to read, templated and shared through version control
    ssytems e.g. GitHub.  Its power and stabilty come from the underlying Python
    libraries that it wraps - docutils, numpy, sympy, matplotlib and pandas.
    
    rivtText is processed by rivtcalc, a Python package. The rivtcalc API
    exposes five methods: R(r-rs), I(i-rs), V(v-rs), T(t-rs), X(rs); where rs
    represents a rivtText string. In interactive mode (e.g. in the IDE VSCode)
    each API method can be treated as a cell (#%%) and evaluated interactively,
    producing utf8 text output. In file mode the rivtText calculation (calc) is
    written to disk as formatted text (utf8) and optionally as a doc or html
    (PDF or HTML). Doc calcs may include private project-specific information
    and potentially copyrighted images that are sourced from unshared folders.
    In contrast, text calcs and rivt input files are generic and shareable under the
    MIT open source license.
    
    rivtText generates it's own calculation output through equations and
    functions, and can incorporate output from other engineering software.
    External sources can be automatically inserted as PDFs, images, text, LaTeX
    and tables. rivtcalc output is a formatted calc file organized in sections.
    Report calcs are assembled from multiple doc calcs with page headings, cover
    and table of contents 
    
    rivtText wraps the reStructuredText (reST) language and native Python code
    and adds a dozen unique commands and tags for simplification and
    readability. Commands generally integrate external files into a calc and are
    triggered with || at the beginning of a line. Tags format output and are
    triggered by [tag]_ or [tag]__ (double underscore) at the end of a line,
    depending on whether a single line or a block of lines are formatted. Within
    a v-rs (values string), the = sign is the command that triggers evaluation
    of an equation.

    API methods ----------------------------------------------------------------
    
    type    method             content and commands
    ======= =================================================================== 
    Repo    R(rs)   any text, ||output, ||project, ||search, ||append 
    Insert  I(rs)   any text, ||text, ||tables, ||image 
    Values  V(rs)   text, =,  ||values, ||lists, ||import, plus the I commands 
    Tables  T(rs)   Python simple statements, plus the I and V commands
    Exclude X(rs)   exclude evaluation - for debugging and review

    tags ----------------------------------------------------------------------

    tags - line               description (user calc input in parenthesis)
    ===============  ==========================================================
    (description) [n]_  : section description and autonumber 
    (description) (|n,n|sub;nosub) [e]_ : equation description, autonumber,format
    (title) [t]_        : table title and autonumber 
    (caption) [f]_      : figure caption and autonumber
    (sympy eq) [s]_     : format sympy equation (latex eq) [x]_ format LaTeX equation 
    (text) [r]_         : right justify text 
    (text) [c]_         : center text
    (text) [l]_         : left justify text (default)
    (text) [#]_         : footnote autonumber 
    (footnote) [foot]_  : footnote description, placed at end of string
    (http://url label) [link]_ : active url with optional label 
    [line]_             : horizontal line - width of page
    [page]_             : new page (PDF) 

    tags - block                          description 
    ================  ===================================================== 
    (text) [r]__         : right justify text block 
    (text) [c]__         : center text block
    (text) [l]__         : left justify text block 
    [literal]__          : literal block 
    [latex]__            : LaTeX block 
    [math]__             : math block

    rivt calcs and supporting files are stored in a specific rivt project folder
    structure designed to simplify file exchange. The three top level folders
    are named calcs, docs, and files (calcs and docs folders are reviewed here).
    The folders can be subfolders of any other folder but must be kept together.
    The calcs folder contains the complete calculation report structure in
    rivtText and other text files and is designed to be publically shared and
    version controlled on a software development platform like Github. Files
    included in the calcs folder include rivtText, LaTex, tables, data,
    equations and functions.

    Each rivt filename begins with the letter c and a four digit calc number:
    
    cddnn_filename.py

    where dd is the division number, nn is the rivt file number within the
    division and cddnn together is the calc number. Within each rivt file
    sections may be further defined. Each division with its and supporting files
    is containted in a subfolder. The file and folder name suffixes, following
    the number and underscore, is chosen by the user. When a rivt report is
    generated, it is organized and formatted using the division, file and
    section structure. 
    
    The entire calcs folder, containing a few component design files hundreds of
    files desinging an entire building, is structured for public sharing and
    editing as a template for related calcs.  rivtText files are Python (.py)
    files start with a calc number. The associated rivt calc text file output
    retains the same calc number and appends the .txt suffix.

    rivtproject_folder
        calcs
            c00_config
                units.py
            c01_loads
                c0101_gravity.py
                c0102_wind.py
                c0101_gravity.txt
                c0102_wind.txt
                wind_table.rst
                material_weights.csv
                code_paragraph.txt
            c02_beams
                c0201_floor.py
                c0202_roof.py
                c0201_floor.txt
                c0202_roof.txt
                beam_formulas.py
        docs
        files

    The docs folder structure mirrors the calcs folder and contains binary data,
    including images and PDF files as well as proprietary project information
    such as client related data. It is typically not publically shared. 
    
    PDF calcs, and complete PDF and HTML rivt reports are written to the docs folder. 

    rivtproject_folder
        calcs
        docs
            d00_config
                pdf_style.sty
                html_config.css
                config.txt
                project_data.sylk
            d01_loads
                image01.jpg
                iamge02.png
                d0101_gravity.pdf
                d0102_wind.pdf
            d02_beams
                image03.jpg
                attachment.pdf
                d0201_floor.pdf
                d0202_roof.pdf
            html
                resources
                    image01.jpg
                    image02.png
                    image03.jpg
                index.html
                d0101_gravity.html
                d0102_wind.html
                d0201_gravity.html
                d0202_wind.html
            rivt_report.pdf
        files

    In the rivt input example provided below, user supplied arguments to
    commands and tags (e.g. filenames) are in parenthesis. Selection arguments
    (e.g. L;R;C) are separated by a semi-colon. Explanatory comments are in
    braces. 
    
    The first line of a rivt file imports the rivtcalc API, genearally followed
    by R(r-rs). R is unique because it occurs first and once.  It sets options
    for repository and output formats. Following R, other methods can
    occur in any order and frequency.  All method invocations start in column 1,
    followed by an optional section title. Subsequent lines in the method must
    be indented 4 spaces which facilitates editor folding, bookmarking and
    legibility. Automatic line rewrapping with hard line breaks is implemented
    in the IDE. Line 80 is standard. Other rivtText formatting conventions
    follow rules enforced by the Python formatter Black.


example rivt calc file  --------------------------------------------------------

# every rivt calc file must start with the following line
from rivtcalc import calc as rv 

rv.R("""Repo method [n]_ 

    The Repo method specifies parameters for repository and output processes and
    includes introductory calculation text. It is always the first method when
    used, and should generally be included although the calc can run using built
    in default values. The first paragraph of R is saved to a README.rst file as
    an overview of the calc when the ||search command is run. 
    
    The ||search command generates a list of search terms found in calcs
    included on a list of calc numbers. Terms found in the calcs are written to
    the README.rst fiel to facilitate repository searches It overwrites any
    existing README file each time it is run.

    The ||output command specifies the type of output, formatting style,
    calculation title, starting page number and temporary file cleanup options.
    The |term setting is used for interactive calc editing. If an output type
    other than |term is specified the entire calc is evaluated and the output is
    written to the specified file type.
    
    The ||project command inserts a project table from information in private
    doc folders. Other private files include image and PDF files read from doc
    folders. The folder structure of rivt keeps confidential and potentially
    copyrighted information separate from shareable, version controlled
    information.

    The ||append command attaches PDF documents to the front or back of a doc.
    Any existing PDF document stored in its corresponding doc folder can be
    attached to the doc. The command can also overlay a title block page
    template on each page of the calc.

    The ||report command collates and assembles PDF docs into a report with page
    numbers, table of contents, and lists of figures and tables. The report
    configuration file is stored in the d0000 folder.

    || output  | rivt.sty;(file.sty) | term;utf;pdf;html;report |  (title) |(page) 
    || project | (project_data.xlsx;.syk,.csv,.txt) | (width),L;R;C 
    || search  | rivt;(searchlist.csv) | all;(calclist.ini) 
    || append  | (file.pdf),(block.pdf) | front;back;block 
    || report  | (configfile.txt)| (reportfilename.pdf) 
    
    """) 
    
rv.I("""Insert method [n]_ 

    The Insert method formats calc information using three commands
    ( ||text, ||table, ||image ) and a dozen tags (summarized above). 
    
    || text | (file.txt) | rivt;plain;indent

    Insert tables from files using the following:
    
    table title  [t]_ 
    || table | (file.csv;.rst) | (60,r;l;c) {max col width,location }

    Insert images from files using the following:
    
    || image | (f1.png) | (50,l;c;r) {scale percent of page width, location} 
    A figure caption [f]_ {centered}

    Insert two images side by side using the following: 
    
    || image | (f1.png) | (35) | image | (f2.png) | (45) {scale percent of page width}
    [a] The first figure caption [f]_ 
    [b] The second figure caption  [f]_

    The tags [x]_ and [s]_ format LaTeX and sympy equations:

    \gamma = \frac{5}{x+y} + 3  [x]_ 
    x = 32 + (y/2)  [s]_

    http://wwww.someurl  (label) [link]_ {formats a URL link}

    """
) rv.V("""Value method [n]_ 

    The Value method assigns values to variables and numerically evaluates
    equations. The = sign triggers the evaluation. Successive rows of values
    that are terminated with a blank line are formatted into a table and saved
    to a file for subsequent use. 

    a1 = 10.1    | unit, alt unit | description 
    d1 = 12.1    | unit, alt unit | description # save {records to csv file - values.csv}

    Equations have the syntx:

    a1 = 3.14*(d1/2)^2   | unit, alt unit  # save {stores to file}

    An equation tag placed above an equation labels it with a description, auto
    numbers it, specifies the printed decimal places in equations and results
    respectively, and whether the equation is printed with substituted numerical
    values. The equation tag precedes the equation and has the syntax,
    
    Area of circle | 2,2 | nosub;sub [e]_ 

    The decimal and substitution formatting options are retained until changed -
    they do not need to be specified each time. The equation tag is optional and
    defaults to the description "equation" if stored.

    The commands ||values, ||list and ||import read values and functions from
    files.  
    
    Values saved from previous calcs can be imported.  The ||values command
    imports all values from a file, where the content in each row follows the
    input order (variable name, value, primary unit, secondary unit,
    description). Selected values may be printed. 

    || values | (file.csv .xlxs .syk) | print([x:y]) {list of rows to print}

    The ||list command imports data from a file, where the first column is the
    variable name and the subsequent csv values make up a vector of values assigned
    to the variable.
    
    || list | (file.csv .xlxs .syk) | [:];([x:y]) {rows to import}
  
    Functions are imported as methods from a Python file.  The function name,
    arguments and doc strings may be included in the calc using the docs;nodocs
    argument. 

    || import | (file.py) | docs;nodocs

    Functions may also be defined in the Table method, if written on a single line.

    """
) rv.T("""Table method [n]_  

    [literal]__
    The Table method generates tables, plots and functions from native Python
    code. The method may include any Python simple statement (single line), rivt
    commands or tags. Any library imported at the top of the calc may be used,
    along with the pandas, numpy, sympy and matplotlib library methods imported
    by rivtcalc. The included libraries are available as:
            pandas pd.method() np.method() mp.method()
    Some common single line Python statements for defining functions or reading
    a file include:
    
    def f1(x,y): print(x,y); return x*y
    
    with open('file.csv', 'r') as f: output = f.readlines()
    
    """
) rv.X("""Exclude Evaluation [n]_

    Excludes evaluation of the string. Used for comments and debugging. 

    """ 
) '''











Check out the :doc:`usage` section for further information, including
how to :ref:`installation` the project.

.. note::

   This project is under active development.

Contents
--------

.. toctree::

   usage
   api
