import subprocess

# Define a mapping between Python and COBOL constructs
translation_map = {
    'print': 'DISPLAY',
    'if': 'IF',
    'else': 'ELSE',
    'for': 'PERFORM VARYING',
    'while': 'PERFORM UNTIL',
    'def': 'PROCEDURE DIVISION.',
    'return': 'EXIT.',
    'True': 'TRUE',
    'False': 'FALSE'
}

# Function to translate Python code to COBOL
def translate_to_cobol(python_code):
    cobol_code = [
        '       IDENTIFICATION DIVISION.',
        '       PROGRAM-ID. SampleProgram.',
        '       DATA DIVISION.',
        '       WORKING-STORAGE SECTION.'
    ]
    for line in python_code.split('\n'):
        # Handle variable assignments
        if '=' in line and '==' not in line:
            var, value = line.split('=')
            var = var.strip()
            value = value.strip().strip('"')
            cobol_code.append(f'       01 {var} PIC X({len(value)}).')
            cobol_code.append(f'       MOVE "{value}" TO {var}.')
        else:
            for py_construct, cobol_construct in translation_map.items():
                line = line.replace(py_construct, cobol_construct)
            cobol_code.append('       ' + line)
    cobol_code.append('       PROCEDURE DIVISION.')
    cobol_code.append('       STOP RUN.')
    return '\n'.join(cobol_code)

# Function to compile and run COBOL code
def compile_and_run_cobol(cobol_code):
    with open('program.cob', 'w') as file:
        file.write(cobol_code)
    
    # Compile the COBOL code
    compile_process = subprocess.run(['cobc', '-x', 'program.cob', '-o', 'program'], capture_output=True, text=True)
    if compile_process.returncode != 0:
        print("Compilation Error:\n", compile_process.stderr)
        return
    
    # Run the compiled COBOL program
    run_process = subprocess.run(['./program'], capture_output=True, text=True)
    if run_process.returncode != 0:
        print("Runtime Error:\n", run_process.stderr)
    else:
        print("Program Output:\n", run_process.stdout)

# Main function to test the translation and execution
def main():
    sample_python_code = """
def greet(name):
    print("Hello, " + name)

greet("World")
"""
    cobol_code = translate_to_cobol(sample_python_code)
    print("COBOL Code:\n", cobol_code)
    compile_and_run_cobol(cobol_code)

# Run the main function
if __name__ == "__main__":
    main()
