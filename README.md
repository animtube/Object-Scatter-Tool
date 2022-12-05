global proc ObjectScatterToolUI()
{
    if(`window -exists "OSTool"` == true)
    deleteUI "OSTool" ;
    window -title "Object Scatter Tool ver.1.0" "OSTool" ;
    columnLayout;
    button -label "1. Set Objects" -width 295 -c "setobjects" ;
    button -label "2. Rivet" -width 295 -c Rivet ;
    button -label "3. Scatter" -width 295 -c "scatter" ;
    button -label "Process Reset" -width 295 -c "reset" ;
    showWindow "OSTool" ;
}
ObjectScatterToolUI() ;

global proc setobjects()
{
    string $objects[] = `ls -sl` ;
    for ($i=0; $i<size($objects); $i++)
        {
            CreateLocator ;
            string $Loc[] = `ls -sl` ;
            rename $Loc[0] ("ScatterObj" + $i) ;
            select -tgl $objects[$i] ;
            parent ;
        }
}

global proc scatter()
{
    string $scloc[] = `ls -sl` ;
    select ("ScatterObj*");
    pickWalk -d up;
    string $obj[] = `ls -sl` ;
    for ($i=0; $i<size($scloc); $i++)
        {
            float $pos[] = `xform -q -ws -t $scloc[$i]`;
            float $rot[] = `xform -q -ws -ro $scloc[$i]`;
            setAttr ( $obj[$i] + ".translate") $pos[0] $pos[1] $pos[2] ;
            setAttr ( $obj[$i] + ".rotate") $rot[0] $rot[1] $rot[2] ;
        }
        select -r $scloc ;
        doDelete ;
        select ("ScatterObj*");
        doDelete ;
}

global proc reset()
{
    select ("pinOutput*");
    doDelete ;
    select ("ScatterObj*");
    doDelete ;
}
