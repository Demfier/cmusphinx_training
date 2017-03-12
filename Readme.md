## Model Details

This model has been trained on the an4 database.<br>

## Results

**SENTENCE ERROR** : 55.4%<br>
**WORD ERROR RATE** : 18.5%<br>

To see a more detailed description of the model result, check the [result folder](result/).

## Errors that I faced & how I countered them

I tried to follow the steps mentioned in the tutorial [here](http://cmusphinx.sourceforge.net/wiki/tutorialam) very closely, but I am a human my friend! Doing mistakes is in our nature! So don't worry if you too did the same mistakes I did, it's completely normal :)<br>

Below is the list of errors I faced and how I counterd them.<br>
**Note:** My first step was to check my log file (located at ```an4/logdir``` folder), google out the errors and see what comes. (Pretty obvious isn't it !)<br>

* _Configuration (e.g. etc/sphinx_train.cfg) not defined_ :<br>
    <pre>
    Running the training
    Configuration (e.g. etc/sphinx_train.cfg) not defined
    Compilation failed in require at /usr/local/lib/sphinxtrain/scripts/000.comp_feat/slave_feat.pl line 51.
    BEGIN failed--compilation aborted at /usr/local/lib/sphinxtrain/scripts/000.comp_feat/slave_feat.pl line 51.
    </pre>
    **Solution:** Set environment variables using the following commands:<br>

    ```export PATH=/usr/local/bin:$PATH``` <br>
    ```export LD_LIBRARY_PATH=/usr/local/lib``` <br>
    ```export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig``` <br>

* _Training failure while decoding the model_ :<br>
    <pre>
    Sphinxtrain path: /usr/local/lib/sphinxtrain
    Sphinxtrain binaries path: /usr/local/libexec/sphinxtrain
    Running the training
    MODULE: 000 Computing feature from audio files
    Extracting features from  segments starting at  (part 1 of 1)
    Extracting features from  segments starting at  (part 1 of 1)
    Feature extraction is done
    MODULE: 00 verify training files
    Phase 1: Checking to see if the dict and filler dict agrees with the phonelist file.
    .
    .
    .
    Skipped:  $ST::CFG_MMIE set to 'no' in sphinx_train.cfg
    MODULE: 65 MMIE Training
    Skipped:  $ST::CFG_MMIE set to 'no' in sphinx_train.cfg
    MODULE: 90 deleted interpolation
    Skipped for continuous models
    MODULE: DECODE Decoding using models previously trained
            Decoding 3 segments starting at 0 (part 1 of 1)
            0%
    This step had 3 ERROR messages and 0 WARNING messages.  Please check the log file for details.
            Aligning results to find error rate
    word_align.pl failed with error code 65280 at /usr/local/lib/sphinxtrain/scripts/decode/slave.pl line 173.
    </pre>

    **Solution:** Configure ```etc/sphinx_train.cfg``` properly: I believe this error arised due to some unset variables in our configuration file like.
    * ${CFG_DIRLABEL}: Set to directory label such as ptm, ci etc..
    * ${CFG_N_TIED_STATES}: Set to number of tied states value. <br>
    (e.g. $DEC_CFG_MODEL_NAME: Set $CFG_EXPTNAME.cd_${CFG_DIRLABEL}_${CFG_N_TIED_STATES} --> $CFG_EXPTNAME.cd_ptm_200)
    * ${CFG_BASE_DIR}: Set to your database directory.
    * ${CFG_EXPTNAME}: Set to your database name. <br>
    (e.g ${CFG_BASE_DIR}/model_architecture/${CFG_EXPTNAME}.tree_questions --> /home/gaurav/python-scripts/py/scripts/sphinx-source2/an4/model_architecture/an4.tree_questions)
* _Training failure while decoding the model_ :<br>
    **Solution**: <br>
    Well something similar came to my terminal like last time, but the reason was different. This time, it couldn't find the language model file. On searching my ```etc/``` folder, I found out that the our lm file was present by the name of ```an4.ug.lm.DMP```. Still not sure where that "ug" came from and I would love to know it. All I did was to change the filename to ```an4.lm.DMP``` and it seemed to do the trick.

## Additional Note:
If I missed any particular error that you faced while training or if you think my solutions are incorrect, feel free to contact me at [sahu.gaurav719@gmail.com](mailto:sahu.gaurav719@gmail.com) or just open up a new issue!
