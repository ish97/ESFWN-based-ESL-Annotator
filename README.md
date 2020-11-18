# ESFWN-based ESL Annotator
**Event Structure Frame-annotated WordNet (ESFWN)** is a knowledge base which includes the offset and the synset number of WordNet and the proper Event Structure Frames (ESF) for all verbs in English. The ESFWN makes it possible to use the information WordNet provides and the ESF for verbs in English text. Its filename is **esfwn_v1.json**.       
**ESFWN-based ESL Annotator** annotates the Event Structure Lexicon (ESL) for each verb in English text. The ESL includes verb itself, verb lemma, the WordNet offset and synset number for linking to WordNet, synonyms and hypernyms, the ESF type and its corresponding ESF. The whole package is in the script_esf folder.     **Installation and usage guidline is shown in readme.md in the "script_esf" folder**.


## ESL Example
   **sentence**: John is **putting** a book on a table.    
   **v.text**: putting    
   **v.lemma**: put    
   **v.offset**: wn:01494310v    
   **v.sem_roles**: [(ARG0, John), (V, put), (ARG1, a book), (ARG2, on a table)]    
   **wn_synset**: put.v.01    
   **synonyms**: [put, set, place, pose, position, lay]    
   **hypernyms**: [move, displace]    
   **esf_type**: CAUSE_MOVE_TO_GOAL    
   **esf**: [{se_num: se1, time: t1, se_type: pre-state, se_form: be (theme, source_location)},    
              {se_num: se2, time: t1, se_type: d-pre-state, se_form: be_not (theme, goal_location)},    
              {se_num: se3, time: t2, se_type: process, se_form: V-ing (agent, source_location, goal_location)},    
              {se_num: se4, time: t3, se_type: post-state, se_form: be (theme, goal_location)}]    
              (**pre-state**: a presupposed state before the maintaining event, **post-state**: an entailed state after the maintaining event)


#### MissOh_esl is an esl-annotated json file for the MissOh script.
