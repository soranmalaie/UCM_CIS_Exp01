# UCM_CIS_Exp01
<!DOCTYPE html>
<html>
    <head>
        <title>Letter_Set</title>
        <script src="https://unpkg.com/jspsych@7.3.1"></script>
        <script src="https://unpkg.com/@jspsych/plugin-html-keyboard-response@1.1.2"></script>
        <script src="https://unpkg.com/@jspsych/plugin-sketchpad@1.0.3"></script>
        <script src="https://unpkg.com/@jspsych/plugin-preload@1.1.2"></script>
        <script src="https://unpkg.com/@jspsych/plugin-survey-html-form@1.0.2"></script>
        <script src="https://unpkg.com/@jspsych/plugin-survey@0.2.1"></script>
        <script src="https://unpkg.com/@jspsych/plugin-instructions@1.1.1"></script>
        <script src="https://unpkg.com/@jspsych/plugin-fullscreen@1.1.1"></script>  
        <script src="https://unpkg.com/@jspsych-contrib/plugin-pipe"></script>      
        <link href="https://unpkg.com/jspsych@7.3.1/css/jspsych.css" rel="stylesheet" type="text/css" />
    </head>
    <body></body>
    <script>

        // experiment ID from DataPipe
        const expID = "ZgNOhrfaWdyb";
        
        // intialize jsPsych
        var jsPsych = initJsPsych({
            on_finish: function() {
                jsPsych.data.displayData();

            }
        });
                
        var timeline = [];

        var tr_duration = 5000;


        //preloading the Venice map
        // var preload = {
        //     type: jsPsychPreload,
        //     images: ['img/Venice_Map.png'],

        // };
        // timeline.push(preload);

        // Change to full screen mode
        var run_fullscreen = {
            type: jsPsychFullscreen,
            fullscreen_mode: true,
        }
        timeline.push(run_fullscreen);

        
        var greeting = {
            type: jsPsychHtmlKeyboardResponse,
            stimulus: "Welcome to the experiment! press any key to start",
        };
        timeline.push(greeting);

        
        //showing instructions 
        var let_set_instruction = {
            type: jsPsychInstructions,
            pages: [
                //<p style="font-size:32px;width:800px;text-align: center">
                //<p style="class=positivefeedback;font-size:24px;max-width:80%">
                '<p style="font-size:32px;font-weight:bold;">Instrucitons<p> ' +
                    '<p style="class=positivefeedback;font-size:24px">In this task, you will see the first two letters of possible words. For each pair of letters,' + 
                    ' you will have 30 seconds to type in as many English words as you can that begin with those letters. ' + 
                    'For example, for <b>lo...</b>, you can write down words such as love, low, lobster, lord,' +
                    ' locomotion, and so on.<p>' + 
                    '<p style="font-size:24px;">You <b>cannot</b> use proper nouns, such as names of places or people (e. g. London, Lora). You can' + 
                    ' be specific in your responses, but do not waste your time by adding different suffixes. For example, '+
                    'love, lover, lovely and loved will be counted just once. Keep in mind that wrong words do not ' + 
                    'have negative points; so, if you doubt whether a word is acceptable, just type it in and continue ' +
                    'searching for more words.',
                    '<p style="font-size:24px;">Before each trial, you will be shown the following map in which you need to deliver some items from the starting point ' + 
                    '(the red dot on the map) to differnet locations. To do this, simply draw a line by holding your keyboard spacebar and' + 
                    ' moving your mouse cursor from the red dot to all the required locations, one by one. ' + 
                    'You should draw the lines through the roads and without crossing over the buliding, and then click on Continue button to ' +
                    'to see the next pair of letters. Do not rush in delivering the items as your 30 seconds time will begin only after ' + 
                    'pushing the Continue button.' + '<br>' //+
                    // '<img src="img/Venice_Map.png"></img>'
                //post_trial_gap: 500 
            ],
            show_clickable_nav: true

                      
        };
        timeline.push(let_set_instruction);
        
        var Venice_prompt = [
            "Dilever the items from the red spot to the following locations:<p> A, B, C, D, E <p>",
                "Dilever the items from the red spot to the following locations:<p> E, F, G, H, I <p>",
                    "Dilever the items from the red spot to the following locations:<p> I, J, K, L, M <p>",
                        "Dilever the items from the red spot to the following locations:<p> M, N, O, P, Q <p>"


        ]
        var Vencie_counter = -1;
        
        var Venice_task = {
            type: jsPsychSketchpad,
            prompt: function(){
                Vencie_counter +=1;
                return Venice_prompt[Vencie_counter]
            },
                prompt_location:'abovecanvas',
            // background_image: 'img/Venice_Map.png', 
            key_to_draw: ' ',
            finished_button_label: 'Continue',
            canvas_width: 1000,
            canvas_height:760,
            canvas_border_width: 3,
            save_strokes: false,
            show_clear_button: false,

            on_start: function() {
                let_counter += 1;
                jsPsych.pluginAPI.clearAllTimeouts();

                // jsPsych.pluginAPI.setTimeout(function() {
                //     jsPsych.finishTrial({Response: "timeout"});
                //     // jsPsych.endCurrentTimeline();
                // }, 15000);
            },

            
        };

        var let_set = ["<b>ab</b>",
                        "<b>da</b>",
                        // "<b>ea</b>",
                        // "<b>gl</b>",
                        // "<b>gr</b>",
                        // "<b>le</b>",
                        // "<b>mi</b>",
                        // "<b>pr</b>",
                        // "<b>st</b>",
                        // "<b>zo</b>",							

                    ];
        var shuffled_let_set = jsPsych.randomization.repeat(let_set, 1);
        var let_counter = -1;

        var let_set_task = {
            type: jsPsychSurveyHtmlForm,
            preamble: function(){
                return shuffled_let_set[let_counter]
            },
            autofocus: 'test-resp-box',
            button_label: "Enter",
            timeline: [
                {html: '<p> <input name="let_respnose" type="text" id="test-resp-box" /></p>',},
                ],
            sample: {
                type: 'fixed-repetitions',
                size:  20
            },
        }
        
        var let_set_procedures = {
            timeline: [Venice_task, let_set_task],
            repetitions: let_set.length,
            
            on_start: function() {
                //jsPsych.pluginAPI.clearAllTimeouts();
                jsPsych.pluginAPI.setTimeout(function() {
                    jsPsych.endCurrentTimeline();
                    jsPsych.finishTrial({Response: "timeout"});
                }, tr_duration);
            }
        }


        var Vencie2_counter = -1;

        
        var Venice_task2 = {
            type: jsPsychSketchpad,
            prompt: function(){
                Vencie2_counter +=1;
                return Venice_prompt[Vencie2_counter]
            },
            // '<p>Imagine there are four itmes at the red spot. Deliver the items to locations Y, M, F, Z, and L, one by one, ' +
            // '(simply draw five lines from the red spots to the locations, without crossing over the buildings)<p>',
            prompt_location:'abovecanvas',
            // background_image: 'img/Venice_Map.png', 
            key_to_draw: 'v',
            finished_button_label: 'Continue',
            canvas_width: 1000,
            canvas_height:760,
            canvas_border_width: 3,
            save_strokes: false,
            show_clear_button: false,

            on_start: function() {
                jsPsych.pluginAPI.clearAllTimeouts();
            }
        };
        
   
        var CRAP_set = ["<b>cottage/swiss/cake</b>",
                        "<b>cream/skate/water</b>",
                         //"<b>loser/throat/spot</b>",
                        // "<b>show/life/row</b>",
                        // "night/wrist/stop",
                        ];

        var shuffled_CRAP_set = jsPsych.randomization.repeat(CRAP_set, 1);
        var CRAP_counter = -1;

        var CRAP_set_task = {
            type: jsPsychSurveyHtmlForm,
            preamble: function(){
                CRAP_counter += 1;
                return shuffled_CRAP_set[CRAP_counter]},

            html: 
                '<p> <input name="CRAP_response" type="text" id="test-resp-box"  style="width:200px;height:50px;" /></p>',

            autofocus: 'test-resp-box',
            button_label: "Enter",
           
            on_start: function() {
                jsPsych.pluginAPI.clearAllTimeouts();
                
                jsPsych.pluginAPI.setTimeout(function() {
                jsPsych.finishTrial({Response: "timeout"});
                    // jsPsych.endCurrentTimeline();
                }, tr_duration);
            },                            
        };

       

        var CRAP_procedures = {
            
            timeline: [Venice_task2, CRAP_set_task],        
            repetitions: CRAP_set.length,

        }

        //  var task_order = [CRAP_procedures, let_set_procedures];
        // var randomized_task_order = jsPsych.randomization.repeat(task_order, 1);
        
        // Use the randomization plugin to randomly choose which function to push first
        var functionOrder = jsPsych.randomization.shuffle([let_set_procedures, CRAP_procedures]);
        
        // Push the functions into the timeline in the randomly determined order
        timeline.push(functionOrder[0]);
        timeline.push(functionOrder[1]);
       

        // add author recognition test here

        var ART = {
            type: jsPsychSurvey,
            pages:
            [
              [
    
                {
                    type: 'multi-select',
                    prompt: "Which of these names are authors? Select all that apply.", 
                    options: [
                      
                          'Patrick Banville', 'Harry Coltheart', 'Virginia Woolf', 'Tony Hillerman',
                          'Kristen Steinke', 'Gary Curwen', 'John Landau', 'Amy R. Baskin',
                          'Ernest Hemingway', 'Herman Wouk', 'Toni Morrison', 'James Clavell',
                          'Clive Cussler', 'Geoffrey Pritchett', 'Harriet Troudeau', 'Salmon Rushdie',
                          'Hiroyuki Oshita', 'Ray Bradbury', 'Roswell Strong', 'Maryann Phillips',
                          'Kurt Vonnegut', 'Jay Peter Holmes', 'J.R.R. Tolkien', 'Scott Alexander',
                          'Anne McCaffrey', 'Christina Johnson', 'Margaret Atwood', 'Ayn Rand',
                          'Elinor Harring', 'Jean M. Auel', 'Seamus Huneven', 'Alex D. Miles',
                          'Sue Grafton', 'Judith Stanley', 'Harper Lee', 'Margaret Mitchell',
                          'Lisa Woodward', 'Gloria McCumber', 'Chris Schwartz', 'Leslie Kraus',
                          'David Harper Townsend', 'James Joyce', 'Walter LeMour', 'Ralph Ellison',
                          'Anna Tsing', 'Robert Ludlum', 'Alice Walker', 'Sidney Sheldon',
                          'T.C. Boyle', 'Larry Applegate', 'Elizabeth Engle', 'Brian Herbert',
                          'Jonathan Kellerman', 'Keith Cartwright', 'T.S. Elliot', 'Sue Hammond',
                          'Cameron McGrath', 'Jackie Collins', 'Marvin Benoit', 'Jared Gibbons',
                          'F. Scott Fitzgerald', 'Umberto Eco', 'Joyce Carol Oates', 'Michael Ondaatje',
                          'A.C. Kelly', 'David Ashley', 'Jessica Ann Lewis', 'Thomas Wolfe',
                          'Peter Flaegerty', 'Jack London', 'Nelson Demille', 'Jeremy Weissman',
                          'Kazuo Ishiguro', 'Seth Bakis', 'Arturo Garcia Perez', 'Willa Cather',
                          'Jane Smiley', 'Padraig Oseaghdha', 'S.L. Holloway', 'J.D. Salinger',
                          'James Patterson', 'E.B. White', 'John Irving', 'Antonia Cialdini',
                          'Martha Farah', 'Giles Mallon', 'Stephen Houston', 'Lisa Hong Chan',
                          'Craig DeLord', 'Raymond Chandler', 'Marcus Lecherou', 'Samuel Beckett',
                          'Nora Ephron', 'Isabel Allende', 'Valerie Cooper', 'Beatrice Dobkin',
                          'Ann Beattie', 'Amy Graham', 'Tom Clancy', 'Wally Lamb',
                          'Stewart Simon', 'Marion Coles Snow', 'Vladimir Nabokov', 'Katherine Kreutz',
                          'Danielle Steel', 'George Orwell', 'Pamela Lovejoy', 'James Michener',
                          'Dick Francis', 'Maya Angelou', 'Vikram Roy', 'William Faulkner',
                          'Ted Mantel', 'Bernard Malamud', 'Saul Bellow', 'Isaac Asimov',
                          'I.K. Nachbar', 'John Grisham', 'Stephen King', 'Lindsay Carter',
                          'Judith Krantz', 'Erich Fagles', 'Elizabeth May Kenyon', 'Paul Theroux',
                          'Thomas Pynchon', 'Walter Dorris', 'Frederick Mundow', 'Francine Preston',
                          'Wayne Fillback', 'Gabriel Garcia Marquez'

 
                    ],
                    columns: 5,
                    name: 'Author', 
                    // correct_response: "Soran",
                     
                }
              ]
            ],
            
            on_start: function() {
            jsPsych.pluginAPI.clearAllTimeouts();
            }
        }
        timeline.push(ART);

           
        //Collecting demographic information
        var demographic_info = {
            type: jsPsychSurvey,
            pages: [
                [
                    {
                        type: 'multi-choice',
                        prompt: "Gender", 
                        options: ['Male','Female','Prefer not to say'],
                        name: 'Gender', 
                        // required: true,
                    },
                    {
                        type: 'text',
                        prompt: "How old are you?",
                        name: 'age', 
                        textbox_columns: 5,
                        // required: true,
                    },
                    {
                        type: 'multi-choice',
                        prompt: "What is your dominant hand",
                        options:['Right', 'Left', 'Ambidextrous (Equally well with both hands)'], 
                        name: 'Handedness', 
                        // required: true,
                    },
                ],
                [
                    {
                        type: 'multi-choice',
                        prompt: "Are you native in English langauge?", 
                        options: ['Yes','No'],
                        name: 'Langauge',
                        // required: true,
                    },
                    {
                        type: 'text',
                        prompt: 'Do you know another language? If yes, please rate it from 0 to 10; with 10 being the heighest',
                        placeholder: 'Example: Spanish (7)',
                        name: 'SecondLanguage'
                    },
                    {
                        type: 'multi-select',
                        prompt: 'What is your race? you can select more than one',
                        options:['White', 'Aferican-American', 'Spanish', 'Asian', 'Other (or prefer not to say)'],
                        name: 'Race',

                    },
                ]
            ],
            title: 'Please enter the following information',
            //button_label_next: 'Continue',
            //button_label_back: 'Previous',
            button_label_finish: 'Submit',
            //show_question_numbers: 'onPage'
        };
        timeline.push(demographic_info);
        


        var Exp_Done = {
            type: jsPsychHtmlKeyboardResponse,
            stimulus: "Experiment is done! Thank you for participating in our project.",
        };
        timeline.push(Exp_Done);


        
        const subject_id = jsPsych.randomization.randomID(10);
        const filename = `${subject_id}.csv`;

        const save_data = {
            type: jsPsychPipe,
            action: "save",
            experiment_id: expID,
            filename: filename,
            data_string: ()=>jsPsych.data.get().csv()
        };
        timeline.push(save_data);


        jsPsych.run(timeline)

    </script>

</html>
