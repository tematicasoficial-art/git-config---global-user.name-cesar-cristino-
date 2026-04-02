import ast
import json
import subprocess
import time

start = time.time()


def string_to_list(input_string):
    try:
        result_list = ast.literal_eval(input_string)
        if isinstance(result_list, list):
            return result_list
        else:
            return None
    except (SyntaxError, ValueError):
        return None


def read_ltl_command():
    ltl_properties = []
    with open('attack_trace_checker/ltl_properties.txt', 'r') as fr:
        lines = fr.readlines()
        for line in lines:
            ltl_properties.append(line.split("\n")[0])

    ltl_properties_count = dict()

    for i in range(len(ltl_properties)):
        ltl_properties_count[i + 1] = 0

    return ltl_properties, ltl_properties_count


def create_inputs():
    all_inp = []
    with open('attack_trace_checker/all_inputs.txt', 'r') as fr:
        lines = fr.readlines()
        for line in lines:
            curr_inp = line.split("\n")[0].strip()
            all_inp.append(curr_inp)
    return list(set(all_inp))


def create_outputs():
    all_outp = []
    with open('attack_trace_checker/all_outputs.txt', 'r') as fr:
        lines = fr.readlines()
        for line in lines:
            curr_outp = line.split("\n")[0].strip()
            all_outp.append(curr_outp)

    return list(set(all_outp))


def create_vars(input_sequence):
    var_str = "VAR\n\n"
    all_inputs = create_inputs()
    all_outputs = create_outputs()
    num_states = len(input_sequence) + 1

    var_input = "input: { "
    for i in range(len(all_inputs)):
        var_input += all_inputs[i]
        if i != len(all_inputs) - 1:
            var_input += ", "

    var_input += " };\n"

    var_output = "output: { "
    for i in range(len(all_outputs)):
        var_output += all_outputs[i]
        if i != len(all_outputs) - 1:
            var_output += ", "

    var_output += ", output_null_action };\n"

    var_state = "state: { "
    for i in range(num_states):
        var_state += "S{}".format(i)# git-config---global-user.name-cesar-cristino-
vhonchito feliz
