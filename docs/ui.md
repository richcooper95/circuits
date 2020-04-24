# Circuits Tool - UI

The following outlines the proposed user interface for a circuits tool encompassing the following functionality:

- Maintain a database of exercises.
  - Data includes:
    - areas trained (list from abs, oblique, arms, legs, cardio);
    - difficulty (easy, medium, hard);
    - date added;
    - number of times included in circuits sessions.
- Construct a new circuits session from the exercise database.
  - Work to a skeleton provided by the user, or revert to a default.
    - A skeleton is a sequence of exercises (specified by area trained) and rests.
  - Allow the user to 'boost' a certain muscle by including more exercises for it.
  - Allow the option to mail the session to a mailer.

The proposed user interface is outlined in the following sections.

## Mainline functionality

### Common semantics

The following behaviours are common across multiple different commands:

- Some operations allow arguments to be passed either on the command line or via a configuration file.
  In the case of the same argument(s) being specified via both mechanisms, the value(s) specified on the command line shall take precedence over those present in the configuration file.

- All user input is verified as soon as possible. This is especially important when writing to the database,
  and to ensure that there are enough exercises of each type in the database to build a custom routine
  skeleton. An error is output to the user if there are any issues with their input.

### session

```text
session [--boost BOOST]
        [--length LENGTH]
        [--group-size GROUP_SIZE]
        [--skeleton SKELETON]
        [--file FILE]
```

This command will create a circuits session with the following settings:

- `-b`, `--boost` - Muscle group to boost during this session.
- `-l`, `--length` - Total number of exercises in this session. Default is 15.
- `-g`, `--group-size` - Size of each group of exercises. Default is 5.
- `-B`, `--build-comps` - List of components to be included only in the targeted build.
- `-s`, `--skeleton` - Skeleton for the circuits session (e.g. 'calr' = core, arms, legs, rest).
- `-f`, `--file` - Specify a skeleton file for the circuits session.

### add

```text
add EXERCISE TYPE DIFFICULTY [--description DESCRIPTION]
```

This command will add the given exercise to the database.

- `EXERCISE` - Name of the exercise.
- `TYPE` - Type of the exercise. Must be a valid type.
- `DIFFICULTY` - Difficulty of the exercise. Must be a valid difficulty.
- `-d`, `--description` - Description of the exercise. Must be enclosed in quotes.

### remove

```text
disable EXERCISE
```

This command will remove the given exercise from the database.

- `EXERCISE` - Name of the exercise to remove from the database.

### show-exercises

```text
show-exercises [--type TYPE]
```

This command will output a table of all exercises in the database.

- `-t`, `--type` - Filter exercises by type.

### show-session

```text
show-session [--file FILE]
```

This command will output the current session to the user.

- `-f`, `--file` - File to read in the session from. Defaults to @@@loc-of-output-file.

## Considerations

The following design aspects were considered:

- Build up the data (exercise sequence, or show command data) and only output to the
   user once it's complete.
  - This allows for extensions such as including in a web app, where the data functions
    can be called directly in response to a query and the data returned in the webpage.
