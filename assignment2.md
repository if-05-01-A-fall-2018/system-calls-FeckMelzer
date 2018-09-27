# Assignment 2: System Calls  
*Author: Feck-Melzer Lukas  
 Class: 3AHIF   
 Assignment #2*

### System Calls Understanding and System Calls Fail
    fork() - create a child process

    creates a new process by duplicating the calling process.  The
    new process is referred to as the child process.  The calling process
    is referred to as the parent process.

    dupliziert den aufrufenden Prozess um damit einen neuen Prozess, einen sogenannten
    "child-process" zu erzeugen. Der aufrufende Prozess wird "parent-process" genannt.
    Beide Prozesse laufen in verschiedenen Speicherorten mit exakt dem selben Inhalt.

    error-causes : e.g. failed to allocate the necessary kernel structures because
                        memory is tight.

________________________________________________________________________________



    stat() - get file status

    These functions return information about a file. No permissions are required on the
    file itself, but-in the case of stat() and lstat() - execute (search) permission is
    required on all of the directories in path that lead to the file. stat() stats the
    file pointed to by path and fills in buf.
    On success, zero is returned. On error, -1 is returned, and errno is set
    appropriately.

    Diese Funktion(en) geben Information über ein File zurück, dafür werden für das
    File selbst keine besonderen Rechte benötigt, allerdings werden für die Suche
    nach einem File durch
    einen bestimmten Pfad Suchrechte benötigt. Der System-Call gibt den Status eines
    Files zurück auf das durch einen Pointer gezeigt wird und füllt den Buffer.
    Bei Erfolg wird 0 zurückgegeben, bei Misserfolg -1 und die Errornummer wird
    passend gesetzt.

    error-causes : e.g. search permission in the given path is denied.
________________________________________________________________________________


    kill() - send signal to a process

    The command kill sends the specified signal to the specified process or process
    group. If no signal is specified, the TERM signal is sent. The TERM signal will
    kill processes which do not catch this signal. For other processes, it may be
    necessary to use the KILL (9) signal, since this signal cannot be caught.
    If the signal is 0 (kill(0)), then no signal is sent, but error checking is
    still performed.

    Der Befehl kill sendet ein bestimmtes Signal zu einem bestimmten Prozess oder zu
    einer Prozessgruppe. Wenn kein Signal bestimmt wird (kill("signal")), wird das
    Signal "TERM" gesendet. Dieses Signal beendet alle Prozesse die dieses nicht
    empfangen. Für alle anderen Prozesse wird es unter Umständen nötig sein, das
    kill(9) Signal zu verwenden, da dieses nicht empfangen werden kann, und somit
    alle Prozesse beendet werden. Ist das angegebene Signal 0 (kill(0)) wird kein
    Signal gesendet, allerdings wird die Fehlerüberprüfung dennoch ausgeführt.

    error-causes : e.g. The process does not have permission to send the signal to
                        any of the target processes.
  ________________________________________________________________________________


    mmap() - maps or unmaps files or devices to or from the memory

    Creates a new mapping in the virtual address space of the calling process. The
    starting address for the new mapping is specified in addr. The length argument
    specifies the length of the mapping.
    If addr is NULL, then the kernel chooses the address at which to create the
    mapping; this is the most portable method of creating a new mapping. If addr
    is not NULL, then the kernel takes it as a hint about where to place the
    mapping.

    Erzeugt ein neues Mapping im virtuellen Adressbereich des aufrufenden Prozesses.
    Die Startadresse für das neue Mapping wird im Parameter addr festgelegt. Der
    Parameter length bestimmt die länge des neuen Mappings.
    Wenn addr NULL ist, sucht sich der Kernel die Adresse aus in der er das neue
    Mapping erzeugt. Ist addr nicht null, so nimmt der Kernel den angegebenen
    Parameter als Hinweis auf einen gewünschten Speichereort.

    error-causes : e.g. The file has been locked, or too much memory has been
                        locked.
  ________________________________________________________________________________

    chmod() - change permissions of a file

    Changes the permissions of the file specified whose pathname is given in path,
    which is dereferenced if it is a symbolic link.
    On NFS file systems, restricting the permissions will immediately influence
    already open files, because the access control is done on the server, but open
    files are maintained by the client. Widening the permissions may be delayed for
    other clients if attribute caching is enabled on them.

    Ändert die Rechte des Files welches im Parameter path durch den Pfad bestimmte
    wurde. Dieses File wird dereferenziert wenn es ein symbolischer Link ist.
    In NFS File-Systemen wird das Beschränken der File-Rechte sofortigen Einfluss
    auch auf bereits geöffnete Files haben, weil die Zugriffskontrolle
    serverseitig geregelt wird, die Files allerdings clientseitig. Das Erweitern
    der File-Rechte kann für andere Clients verzögert werden wenn deren Attribut-
    Caching läuft.

    error-causes : e.g. path points outside your accessible address space.
  ________________________________________________________________________________

    waitpid() - waiting for process to change state

    Is used to wait for state changes in a child of the calling process, and
    obtain information about the child whose state has changed. A state change is
    considered to be: the child terminated; the child was stopped by a signal; or
    the child was resumed by a signal. In the case of a terminated child,
    performing a wait allows the system to release the resources associated with
    the child; if a wait is not performed, then the terminated child remains in a
    "zombie" state.

    Wird verwendet um auf Statusänderungen im child-prozess des Aufrufprozesses
    zu warten und Informationen des child-prozesses zu ermitteln.
    Eine Statusveränderung kann sein:
    + der child-prozess wurde terminiert
    + der child-prozess wurde durch ein signal gestoppt
    + der child-prozess wurde durch ein signal wieder gestartet
    Im Falle eines terminierten child-prozesses erlaubt das warten dem system das
    freigeben der vom child-prozess genutzten Ressourcen. Wird ein wait nicht
    durchgeführt verbleibt der child-prozess in einem sogenannten "Zombie" Status.

    The value of pid can be:

    + <-1 meaning wait for any child process whose process group ID is equal to the
        absolute value of pid.

    + -1 meaning wait for any child process.

    + 0 meaning wait for any child process whose process group ID is equal to
        that of the calling process.

    + > 0 meaning wait for the child whose process ID is equal to the value of pid.


    error-causes: e.g. The calling process does not have any unwaited-for children.
________________________________________________________________________________
    A trap instruction is performed to change from user mode to kernel mode.

    Example:
    A client goes to the drugstore at its nighttime opening hours and rings the
    bell, to inform the employee he needs something. In the moment the client tells
    the employee wich pharmacy products he exactly needed, a
    "real-life-trap-instruction" happens, because obviously the client can't go
    and get his drugs by himself. So the situation "changes into kernel mode"
    as the employee is getting the pharmacies for his client. The moment he is
    giving the medicine to the client, the "system-call" is finished and the
    situation "changes back into user-mode".
