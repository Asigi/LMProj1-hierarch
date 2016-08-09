

var globArray = []; //create empty array.


/* Object */
function hierOb(theName) {
    this.myName = theName;
    this.myChildren = [];
}

/* This is a function for the hierOb object type.
    It adds a child to the list of children for "this" hierOb.
    Parameter theChild is a string */
hierOb.prototype.addChild = function(theChild) {
    var child = new hierOb(theChild);
    this.myChildren.push(child);
    return child;
}



/*
*   create a printer function that prints globArray the way it should be
*
*   Currently prints 4 levels.
        EX:
            LV: 0,1,2,3
                0,1
                0,1,2
                0,1
*/
function globPrinter() {

    var strOutput = "";

    for(var i = 0; i < globArray.length; i++) {
            strOutput += "level 0";
            var obj = globArray[i];
            strOutput += obj.myName;
            for (var j=0; j<obj.myChildren.length; j++) {
                strOutput += "level 1";
                var obj1 = obj.myChildren[j];
                strOutput += obj1.myName;
                for (var k=0; k<obj1.myChildren.length; k++) {
                    strOutput += "level 2";
                    var obj2 = obj1.myChildren[k];
                    strOutput += obj2.myName;
                    for (var l=0; l<obj2.myChildren.length; l++) {
                        strOutput += "level 3";
                        var obj3 = obj2.myChildren[l];
                        strOutput += obj3.myName;
                    }
                }
            }
    }
}







/* Finds if the base-hierachy-element is in globArray.
    Returns -1 if the name passed in does not match any names in globArray.
    Otherwise returns everything. */
function baseChecker(theName) {

    if (globArray.length === 0) {
        return -1;
    }
    for (var i = 0; i < globArray.length; i++) {
        if (globArray[i].myName === theName) {
            return i;
        }
    }

    return -1; //name was not in array.
}


/** Adds objects to hierarchy
 *
 */
function childTree(theParent, theChildArray) {
    //add the first thing in theChildArray as a child to theParent. Note: call addChild function on theParent.
    var childOB = theParent.addChild(theChildArray[0]); //the child hierOb was returned.
    //then add the next thing in theChildArray as a child to that child that was just added.
    if (theChildArray.length > 1) {
        for (var i = 1; i < theChildArray.length; i++) { //loop the array until all children are added.
            childOB = childOB.addChild(theChildArray[i]); //overwrites the childOB variable with the new child.
        }
    }
}


/*Takes in a semicolon seperated string which will
    be handled such that a hierarchy is formed in globArray
    with the use of the hierOb object type.
*/
function hierMaker(theString) {
    var splitStr = theString.split(";");
    var splitLength = splitStr.length;

    var position = baseChecker(splitStr[0])
    if (position >= 0) { //if the name is already in list

        splitLength--;
        if (splitLength > 0) { //just making sure that theString parameter was not just one word.

            var startingOb = globArray[position];
            var childList = startingOb.myChildren;

            var keepGoing = true;
            wLoop: while (keepGoing === true) {
                if (childList.length > 0) {
                    for (var i = 0; i < childList.length; i++) {
                        var matchChild = null;

                        if (splitStr[splitStr.length - splitLength] === childList[i].myName) { //If you find a match...

                            //this one is already in the list. Try seeing if you can go down another step.
                            matchChild = childList[i];
                            //!!! NOTE THAT THE FOR LOOPS upper limit variable is changed here.
                            childList = childList[i].myChildren;



                            if (childList.length === 0 && splitLength <= 0) { //checking the NEW childList //this is needed b/c splitLength is changed within forloop
                                keepGoing = false;
                                break wLoop;

                            } else if (childList.length === 0){ //BASE CASE
                                childTree(matchChild, splitStr.slice(splitStr.length - splitLength + 1, splitStr.length));
                                keepGoing = false;
                                break wLoop;

                            } else { //match was found and this match has a child list so check that list for the next match.

                                //reset i to 0
                                i = 0;
                                //move down the splitStr
                                splitLength--;

                                continue;
                            }

                        } else if ((i +=1) >= childList.length) { //reached the end of childlist and found no match.

                            //add a new child to this childList.
                            var newChild = startingOb.addChild(splitStr[splitStr.length - splitLength]);

                            //move onto adding the rest of splitStr
                            childTree(newChild, splitStr.slice(splitStr.length - splitLength + 1, splitStr.length));
                        }
                    }
                }
            }
        }
    } else {
        //Add the name to the highest level of the hieracrchy.
        globArray[globArray.length] = new hierOb(theString);
    }
}












//main


//TODO add one list to the globArray by passing it into hierMaker. Print globArray using printer function.
hierMaker("Pokemon;Grass;Bulbasaur");
globPrinter();

//TODO add the exact same thing. Print.

//TODO add another list with a different base. Print.

//TODO add another list with a same base. Print.


















//end
