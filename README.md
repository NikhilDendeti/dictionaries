# dictionaries
unit_details = [
    {
        "unit_id": "unit_1",
        "topic_ids": ["topic_1", "topic_2"],
    },
    {
        "unit_id": "unit_2",
        "topic_ids": ["topic_3"],
    }
]

topic_details = [
    {
        "topic_id": "topic_1",
        "topic_name": "Topic 1",
        "course_ids": ["course_1"],
    },
    {
        "topic_id": "topic_2",
        "topic_name": "Topic 2",
        "course_ids": ["course_2"],
    },
    {
        "topic_id": "topic_3",
        "topic_name": "Topic 3",
        "course_ids": ["course_3", "course_4"],
    }
]

course_details = [
    {
        "course_id": "course_1",
        "course_name": "Course 1",
        "program_ids": ["program_1"],
    },
    {
        "course_id": "course_2",
        "course_name": "Course 2",
        "program_ids": ["program_2"],
    },
    {
        "course_id": "course_3",
        "course_name": "Course 3",
        "program_ids": ["program_3"],
    },
    {
        "course_id": "course_4",
        "course_name": "Course 4",
        "program_ids": ["program_1", "program_2"],
    }
]

program_details = [
    {
        "program_id": "program_1",
        "program_name": "Program 1",
        "category_name_enum": "Category 1",
    },
    {
        "program_id": "program_2",
        "program_name": "Program 2",
        "category_name_enum": "Category 2",
    },
    {
        "program_id": "program_3",
        "program_name": "Program 3",
        "category_name_enum": "Category 3",
    },
    {
        "program_id": "program_4",
        "program_name": "Program 4",
        "category_name_enum": "Category 4",
    }
]


def topics_dictionary(topic_details):
    topic_id_dict = {}
    for topic in topic_details:
        topic_id_dict[topic["topic_id"]] = topic
    return topic_id_dict


def course_dictionary(course_details):
    course_id_dict = {}
    for course in course_details:
        course_id_dict[course["course_id"]] = course
    return course_id_dict


def program_dictionary(program_details):
    program_id_dict = {}
    for program in program_details:
        program_id_dict[program["program_id"]] = program
    return program_id_dict


def topics_fun(unit_topics, topic_id_dict, course_id_dict, program_id_dict):
    topics_list = []
    for topic_id in unit_topics:
        topic = topic_id_dict[topic_id]
        topic_dict = {"topic_id": topic_id, "topic_name": topic["topic_name"]}
        courses = []
        for course_id in topic["course_ids"]:
            courses.append(course_fun(course_id, course_id_dict, program_id_dict))
        topic_dict["courses"] = courses
        topics_list.append(topic_dict)
    return topics_list


def course_fun(course_id, course_id_dict, program_id_dict):
    course = course_id_dict[course_id]
    course_dict = {"course_id": course_id, "course_name": course["course_name"]}
    program_list = []

    for program_id in course["program_ids"]:
        program_list.append(program_id_dict[program_id])

    course_dict["programs"] = program_list
    return course_dict


def process_unit(unit, topic_id_dict, course_id_dict, program_id_dict):
    unit_dict = {"unit_id": unit["unit_id"]}
    unit_dict["topics"] = topics_fun(unit["topic_ids"], topic_id_dict, course_id_dict, program_id_dict)
    return unit_dict


def process_units(unit_details, topic_id_dict, course_id_dict, program_id_dict):
    response = []
    for unit in unit_details:
        response.append(process_unit(unit, topic_id_dict, course_id_dict, program_id_dict))
    return response


topic_id_dict = topics_dictionary(topic_details)
course_id_dict = course_dictionary(course_details)
program_id_dict = program_dictionary(program_details)

response = process_units(unit_details, topic_id_dict, course_id_dict, program_id_dict)
import json

print(json.dumps(response, indent=4))
