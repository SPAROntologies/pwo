## Competency Questions

PWO can be used for answering several questions related to publication workflows.
In the following subsections, some of them are introduced together with their respective SPARQL queries. 

The prefixes that are used in all the SPARQL queries provided below are defined as follows:

    PREFIX pwo: <http://purl.org/spar/pwo/>
    PREFIX taskex: <http://www.ontologydesignpatterns.org/cp/owl/taskexecution.owl#>
    PREFIX part: <http://www.ontologydesignpatterns.org/cp/owl/participation.owl#>
    PREFIX dcterms: <http://purl.org/dc/terms/>
    PREFIX frbr: <http://purl.org/vocab/frbr/core#>
    PREFIX tisit: <http://www.ontologydesignpatterns.org/cp/owl/timeindexedsituation.owl#>
    PREFIX ti: <http://www.ontologydesignpatterns.org/cp/owl/timeinterval.owl#>

### CQ1

What is the ordered sequence of steps in a workflow, including their required inputs and produced outputs?

    SELECT ?workflow ?step ?nextStep ?requiredInput ?producedOutput
    WHERE {
        ?workflow a pwo:Workflow ;
            pwo:hasStep ?step .
        ?step a pwo:Step .
        OPTIONAL { ?step pwo:hasNextStep ?nextStep . }
        OPTIONAL { ?step pwo:needs ?requiredInput . }
        OPTIONAL { ?step pwo:produces ?producedOutput . }
    }

### CQ2

Which actions execute a workflow step, and who or what participates in them?

    SELECT ?step ?action ?actionDescription ?participant
    WHERE {
        ?step a pwo:Step ;
            taskex:isExecutedIn ?action .
        ?action a pwo:Action .
        OPTIONAL { ?action dcterms:description ?actionDescription . }
        OPTIONAL { ?action part:hasParticipant ?participant . }
    }

### CQ3

What are the start and end times of actions involved in a workflow execution?

    SELECT ?workflowExecution ?action ?startDate ?endDate
    WHERE {
        ?workflowExecution a pwo:WorkflowExecution ;
            pwo:involvesAction ?action .
        ?action tisit:atTime ?timeInterval .
        OPTIONAL { ?timeInterval ti:hasIntervalStartDate ?startDate . }
        OPTIONAL { ?timeInterval ti:hasIntervalEndDate ?endDate . }
    }