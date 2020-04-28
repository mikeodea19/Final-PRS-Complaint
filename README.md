# Final-PRS-Complaint
# Prepared by Community.lawyer on 4/28/2020
# Compatible with Docassemble version 0.5.4
---
modules:
  - docassemble.base.util
  - .everything
  - .debug
---
mandatory: True
code: |
  from datetime import datetime
  import pytz, flask, re
  from docassemble.base.functions import get_uid
  from docassemble.base.util import device
  import secrets
  import uuid

  # interview id

  interview_id = 9723

  # valid subdomain

  ___pattern_1 = re.compile('https://(.*?).(qa.)?community.lawyer')
  ___pattern_2 = re.compile('https://(.*?).(qa.)?app.law')
  ___match = ___pattern_1.match(flask.request.base_url) or ___pattern_2.match(flask.request.base_url)

  if ___match:
    ___current_subdomain = ___match.group(1)

    if not (___current_subdomain == 'app' or ___current_subdomain == 'qa-app'):
      ___subdomain_is_correct = 'dabr0010' == ___current_subdomain

      if not ___subdomain_is_correct:
        command('exit', url="https://community.lawyer/404")

  elif False:
    ___pattern = re.compile('https://')
    ___match = ___pattern.match(flask.request.base_url)

    if not ___match:
      command('exit', url="https://community.lawyer/404")

  else:
    command('exit', url="https://community.lawyer/404")

  undefine('___pattern')
  undefine('___match')

  # today

  define("Today", as_datetime(datetime.now(pytz.timezone(get_default_timezone()))))


  # answers session id

  define("___answers_session_id", get_uid())

  # da session id

  define('session_id', user_info().session)

  # cl session id (exposed to the user as a variable)

  define('cl_session_id', secrets.token_hex())

  # url args from clio custom actions

  ___subject_url = url_args.get("subject_url")
  if (___subject_url):
    if ("/api/v4/contacts/" in ___subject_url):
      define("clio_contact_id", ___subject_url.replace("/api/v4/contacts/", ''))
    elif ("/api/v4/matters/" in ___subject_url):
      ___matter_id = ___subject_url.replace("/api/v4/matters/", '')
      define("clio_matter_id", ___matter_id)
      ___matter_url = "https://app.clio.com/api/v4/matters/%s?fields=client{id,name}" % (___matter_id)
      ___clio_matters_api_response = requests.get(___matter_url, headers=get_clio_access_headers()).json().get('data')
      ___contact_id = ___clio_matters_api_response.get('client').get('id')
      define("clio_contact_id", ___contact_id)

  # portal client id

  ___Portal_client_id = url_args.get("Portal_client_id")
  if (___Portal_client_id):
    define("Portal_client_id", ___Portal_client_id)

  # fallback key value for cldb
  define("___cldb_key_value_uuid", str(uuid.uuid4()))

  # memoized values

  define("___memoized_values", {})

  # server host

  define("___server_host", "https://app.app.law")

  # type map

  type_map = { "Portal_client_id": "string", "___shortcut_4_false": "boolean", "___ivn_4": "boolean", "Today": "date", "___ivn_80": "string", "___shortcut_132_false": "boolean", "___ivn_132": "boolean", "___shortcut_8_false": "boolean", "___ivn_8": "boolean", "___ivn_26": "string", "___ivn_14": "string", "___ivn_121": "string", "___ivn_17": "string", "___ivn_27": "string", "___shortcut_28_choice": "boolean", "___shortcut_30_choice": "boolean", "___shortcut_29_choice": "boolean", "___shortcut_31_choice": "boolean", "___shortcut_32_choice": "boolean", "___shortcut_33_choice": "boolean", "___shortcut_130_choice": "boolean", "___shortcut_34_choice": "boolean", "___shortcut_35_choice": "boolean", "___shortcut_36_choice": "boolean", "___shortcut_37_choice": "boolean", "___ivn_11": "string", "___ivn_18": "string", "___ivn_140": "string", "___shortcut_142_choice": "boolean", "___shortcut_143_choice": "boolean", "___shortcut_146_choice": "boolean", "___shortcut_141_choice": "boolean", "___shortcut_145_choice": "boolean", "___shortcut_144_choice": "boolean", "___ivn_15": "string", "___ivn_13": "string", "___ivn_16": "string", "___ivn_22": "string", "___shortcut_53_false": "boolean", "___ivn_53": "boolean", "___ivn_55": "string", "___shortcut_56_choice": "boolean", "___shortcut_57_choice": "boolean", "___shortcut_58_choice": "boolean", "___shortcut_59_choice": "boolean", "___shortcut_24_false": "boolean", "___ivn_24": "boolean", "___ivn_147": "string", "___ivn_123": "string", "___ivn_109": "string", "___ivn_49": "string", "___ivn_108": "string", "___ivn_107": "string", "___ivn_111": "string", "___ivn_128": "string", "___ivn_110": "string", "___ivn_126": "string", "___ivn_129": "string", "___ivn_127": "string", "___ivn_82": "string", "___ivn_51": "string", "___shortcut_96_false": "boolean", "___ivn_96": "boolean", "cl_session_id": "string", "___ivn_101": "signature", "___ivn_78": "string", "___ivn_120": "string", "___ivn_115": "string", "___shortcut_6_false": "boolean", "___ivn_6": "boolean", "___ivn_116": "string", "___ivn_114": "string", "___ivn_113": "string", "___ivn_118": "string", "___ivn_117": "string", "___ivn_122": "string", "___ivn_135": "date", "___ivn_61": "string", "___shortcut_63_choice": "boolean", "___shortcut_65_choice": "boolean", "___shortcut_64_choice": "boolean", "___shortcut_66_choice": "boolean", "___shortcut_62_choice": "boolean", "___shortcut_2_false": "boolean", "___ivn_2": "boolean" }

  # authorization

  define("___authorized", "aaBa6VXW2AdhkXIhec5zvSCwy" == str(url_args.get('key')))

  # device

  ___user_device = device().browser.family
  if ___user_device == "IE":
    log("This app may not run as expected on Internet Explorer - please consider using a more modern browser, such as those found <a href='http://outdatedbrowser.com/en'>here</a>.", "warning")

  # table definitions
  
  
  
  
  

  # multi-user
  multi_user = True

  # display mapping
  ___display_mapping = { 'Portal_client_id': "Portal_client_id", '___shortcut_4_false': "Suspended_felony is false", '___ivn_4': "Suspended_felony is true", 'Today': "Today", '___ivn_80': "action_taken_by_parent", '___shortcut_132_false': "consent_to_send_complaint_to_school is false", '___ivn_132': "consent_to_send_complaint_to_school is true", '___shortcut_8_false': "disability_suspension_time is false", '___ivn_8': "disability_suspension_time is true", '___ivn_26': "filer_accommodations", '___ivn_14': "filer_apt_number", '___ivn_121': "filer_city", '___ivn_17': "filer_email_address", '___ivn_27': "filer_first_language", '___shortcut_28_choice': "filer_first_language is \"Arabic", '___shortcut_30_choice': "filer_first_language is \"Cape Verdean", '___shortcut_29_choice': "filer_first_language is \"Chinese", '___shortcut_31_choice': "filer_first_language is \"English", '___shortcut_32_choice': "filer_first_language is \"Haitian", '___shortcut_33_choice': "filer_first_language is \"Khmer", '___shortcut_130_choice': "filer_first_language is \"Other Filer  Language", '___shortcut_34_choice': "filer_first_language is \"Portuguese", '___shortcut_35_choice': "filer_first_language is \"Russian", '___shortcut_36_choice': "filer_first_language is \"Spanish", '___shortcut_37_choice': "filer_first_language is \"Vietnamese", '___ivn_11': "filer_full_name", '___ivn_18': "filer_phone_number", '___ivn_140': "filer_role_in_student_suspension", '___shortcut_142_choice': "filer_role_in_student_suspension is \"Advocate", '___shortcut_143_choice': "filer_role_in_student_suspension is \"ESE Assigned Surrogate", '___shortcut_146_choice': "filer_role_in_student_suspension is \"Other", '___shortcut_141_choice': "filer_role_in_student_suspension is \"Parent", '___shortcut_145_choice': "filer_role_in_student_suspension is \"School Employee", '___shortcut_144_choice': "filer_role_in_student_suspension is \"Student", '___ivn_15': "filer_state_live", '___ivn_13': "filer_street_name", '___ivn_16': "filer_zip_code", '___ivn_22': "first_language_not_english", '___shortcut_53_false': "group_of_students is false", '___ivn_53': "group_of_students is true", '___ivn_55': "group_of_students_category", '___shortcut_56_choice': "group_of_students_category is \"Classroom", '___shortcut_57_choice': "group_of_students_category is \"Instructional", '___shortcut_58_choice': "group_of_students_category is \"Program", '___shortcut_59_choice': "group_of_students_category is \"School Wide", '___shortcut_24_false': "help_communicating is false", '___ivn_24': "help_communicating is true", '___ivn_147': "other_role", '___ivn_123': "parent_city_state_zip_student", '___ivn_109': "parent_language_student", '___ivn_49': "parent_name_student", '___ivn_108': "parent_phone_student", '___ivn_107': "parent_street_address_student", '___ivn_111': "school__address", '___ivn_128': "school__employee_email", '___ivn_110': "school_district", '___ivn_126': "school_employee_name", '___ivn_129': "school_employee_phone", '___ivn_127': "school_employee_title", '___ivn_82': "school_fix_situation", '___ivn_51': "school_name", '___shortcut_96_false': "send_complaint_to_dept is false", '___ivn_96': "send_complaint_to_dept is true", 'cl_session_id': "session_id", '___ivn_101': "signature", '___ivn_78': "situation_description_one", '___ivn_120': "situation_description_two", '___ivn_115': "student_age", '___shortcut_6_false': "student_disabilities is false", '___ivn_6': "student_disabilities is true", '___ivn_116': "student_gender", '___ivn_114': "student_grade", '___ivn_113': "student_name", '___ivn_118': "student_primary_language", '___ivn_117': "student_street_address", '___ivn_122': "student_town_state_zip", '___ivn_135': "today_date", '___ivn_61': "type_of_school_program", '___shortcut_63_choice': "type_of_school_program is \"General Education", '___shortcut_65_choice': "type_of_school_program is \"Home School", '___shortcut_64_choice': "type_of_school_program is \"Home", '___shortcut_66_choice': "type_of_school_program is \"Special Education", '___shortcut_62_choice': "type_of_school_program is \"number_program", '___shortcut_2_false': "welcome_page is false", '___ivn_2': "welcome_page is true" }
  ___user_defined_names = list(___display_mapping.keys())

  # url/env variables
  
  
---
mandatory: |
  not ___authorized
question: Unauthorized
subquestion: You have accessed this app without the required key. To get this key, click 'Copy shareable link' from your apps page, or contact the individual or group who provided this link to you.
---
features:
	debug: False
	inverse navbar: False
	javascript:
		- https://community.lawyer/static/system_d-35.js
	css:
		- https://community.lawyer/static/styles/brooklyn-6.css
		- https://community.lawyer/static/styles/spinner.css
---
metadata:
	title: |
		PRS Complaint
---
initial: True
code: |
	set_language('en')
	set_locale(currency_symbol='$')
---
initial: True
code: |
  define('vars', {})
---
id: static_richtext_definitions
mandatory: True
code: |
  # static definitions
---
initial: True
code: |
	def typecast_as_file(_arg):
	  # expects a dict like { url, filename }
	  arg = augment(_arg)
	  if is_undefined(arg):
	    return arg
	  the_file = DAFile()
	  the_file.initialize(filename=arg.wrapped['filename'])
	  the_file.from_url(arg.wrapped['url'])
	  the_file.retrieve()
	  return the_file


	def ___define_until_idempotent():
		___define_immutable_until_idempotent()
		___define_continuous_until_idempotent()

	def ___define_immutable_until_idempotent():
		original_dict = None
		while (original_dict) != (slice_dict(cl_all_variables(), ___user_defined_names)):
			original_dict = slice_dict(cl_all_variables(), ___user_defined_names)
			___define_immutable()
			define('vars', {})

	def ___define_continuous_until_idempotent():
		original_dict = None
		while (original_dict) != (slice_dict(cl_all_variables(), ___user_defined_names)):
			original_dict = slice_dict(cl_all_variables(), ___user_defined_names)
			___define_continuous()
			define('vars', {})

	def ___define_immutable():
		vars = cl_all_variables()






		if ((not ("___shortcut_4_false" in vars)) and (("___ivn_4" in vars))):
				define("___shortcut_4_false", ((primitive_value(((augment(___ivn_4) if ("___ivn_4" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_132_false" in vars)) and (("___ivn_132" in vars))):
				define("___shortcut_132_false", ((primitive_value(((augment(___ivn_132) if ("___ivn_132" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_8_false" in vars)) and (("___ivn_8" in vars))):
				define("___shortcut_8_false", ((primitive_value(((augment(___ivn_8) if ("___ivn_8" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_28_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_28_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Arabic")))))))
		if ((not ("___shortcut_30_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_30_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Cape Verdean")))))))
		if ((not ("___shortcut_29_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_29_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Chinese")))))))
		if ((not ("___shortcut_31_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_31_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("English")))))))
		if ((not ("___shortcut_32_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_32_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Haitian")))))))
		if ((not ("___shortcut_33_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_33_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Khmer")))))))
		if ((not ("___shortcut_130_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_130_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Other Filer  Language")))))))
		if ((not ("___shortcut_34_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_34_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Portuguese")))))))
		if ((not ("___shortcut_35_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_35_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Russian")))))))
		if ((not ("___shortcut_36_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_36_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Spanish")))))))
		if ((not ("___shortcut_37_choice" in vars)) and (("___ivn_27" in vars))):
				define("___shortcut_37_choice", ((primitive_value(((augment(___ivn_27) if ("___ivn_27" in vars) else Undefined()).string_eq(augment("Vietnamese")))))))
		if ((not ("___shortcut_142_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_142_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("Advocate")))))))
		if ((not ("___shortcut_143_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_143_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("ESE Assigned Surrogate")))))))
		if ((not ("___shortcut_146_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_146_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("Other")))))))
		if ((not ("___shortcut_141_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_141_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("Parent")))))))
		if ((not ("___shortcut_145_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_145_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("School Employee")))))))
		if ((not ("___shortcut_144_choice" in vars)) and (("___ivn_140" in vars))):
				define("___shortcut_144_choice", ((primitive_value(((augment(___ivn_140) if ("___ivn_140" in vars) else Undefined()).string_eq(augment("Student")))))))
		if ((not ("___shortcut_53_false" in vars)) and (("___ivn_53" in vars))):
				define("___shortcut_53_false", ((primitive_value(((augment(___ivn_53) if ("___ivn_53" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_56_choice" in vars)) and (("___ivn_55" in vars))):
				define("___shortcut_56_choice", ((primitive_value(((augment(___ivn_55) if ("___ivn_55" in vars) else Undefined()).string_eq(augment("Classroom")))))))
		if ((not ("___shortcut_57_choice" in vars)) and (("___ivn_55" in vars))):
				define("___shortcut_57_choice", ((primitive_value(((augment(___ivn_55) if ("___ivn_55" in vars) else Undefined()).string_eq(augment("Instructional")))))))
		if ((not ("___shortcut_58_choice" in vars)) and (("___ivn_55" in vars))):
				define("___shortcut_58_choice", ((primitive_value(((augment(___ivn_55) if ("___ivn_55" in vars) else Undefined()).string_eq(augment("Program")))))))
		if ((not ("___shortcut_59_choice" in vars)) and (("___ivn_55" in vars))):
				define("___shortcut_59_choice", ((primitive_value(((augment(___ivn_55) if ("___ivn_55" in vars) else Undefined()).string_eq(augment("School Wide")))))))
		if ((not ("___shortcut_24_false" in vars)) and (("___ivn_24" in vars))):
				define("___shortcut_24_false", ((primitive_value(((augment(___ivn_24) if ("___ivn_24" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_96_false" in vars)) and (("___ivn_96" in vars))):
				define("___shortcut_96_false", ((primitive_value(((augment(___ivn_96) if ("___ivn_96" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_6_false" in vars)) and (("___ivn_6" in vars))):
				define("___shortcut_6_false", ((primitive_value(((augment(___ivn_6) if ("___ivn_6" in vars) else Undefined()).boolean_eq(augment(False)))))))
		if ((not ("___shortcut_63_choice" in vars)) and (("___ivn_61" in vars))):
				define("___shortcut_63_choice", ((primitive_value(((augment(___ivn_61) if ("___ivn_61" in vars) else Undefined()).string_eq(augment("General Education")))))))
		if ((not ("___shortcut_65_choice" in vars)) and (("___ivn_61" in vars))):
				define("___shortcut_65_choice", ((primitive_value(((augment(___ivn_61) if ("___ivn_61" in vars) else Undefined()).string_eq(augment("Home School")))))))
		if ((not ("___shortcut_64_choice" in vars)) and (("___ivn_61" in vars))):
				define("___shortcut_64_choice", ((primitive_value(((augment(___ivn_61) if ("___ivn_61" in vars) else Undefined()).string_eq(augment("Home")))))))
		if ((not ("___shortcut_66_choice" in vars)) and (("___ivn_61" in vars))):
				define("___shortcut_66_choice", ((primitive_value(((augment(___ivn_61) if ("___ivn_61" in vars) else Undefined()).string_eq(augment("Special Education")))))))
		if ((not ("___shortcut_62_choice" in vars)) and (("___ivn_61" in vars))):
				define("___shortcut_62_choice", ((primitive_value(((augment(___ivn_61) if ("___ivn_61" in vars) else Undefined()).string_eq(augment("number_program")))))))
		if ((not ("___shortcut_2_false" in vars)) and (("___ivn_2" in vars))):
				define("___shortcut_2_false", ((primitive_value(((augment(___ivn_2) if ("___ivn_2" in vars) else Undefined()).boolean_eq(augment(False)))))))


	def ___define_continuous():
		vars = cl_all_variables()


---
initial: True
code: |
	___define_until_idempotent()
	vars = cl_all_variables()
---
initial: True
code: |
	def passive_possible_variables():
		vars = cl_all_variables()
		generated_names = merge_two_dicts(vars, { 'Portal_client_id': Portal_client_id if ("Portal_client_id" in vars) else '', '___shortcut_4_false': ___shortcut_4_false if ("___shortcut_4_false" in vars) else '', '___ivn_4': ___ivn_4 if ("___ivn_4" in vars) else '', 'Today': Today if ("Today" in vars) else '', '___ivn_80': ___ivn_80 if ("___ivn_80" in vars) else '', '___shortcut_132_false': ___shortcut_132_false if ("___shortcut_132_false" in vars) else '', '___ivn_132': ___ivn_132 if ("___ivn_132" in vars) else '', '___shortcut_8_false': ___shortcut_8_false if ("___shortcut_8_false" in vars) else '', '___ivn_8': ___ivn_8 if ("___ivn_8" in vars) else '', '___ivn_26': ___ivn_26 if ("___ivn_26" in vars) else '', '___ivn_14': ___ivn_14 if ("___ivn_14" in vars) else '', '___ivn_121': ___ivn_121 if ("___ivn_121" in vars) else '', '___ivn_17': ___ivn_17 if ("___ivn_17" in vars) else '', '___ivn_27': ___ivn_27 if ("___ivn_27" in vars) else '', '___shortcut_28_choice': ___shortcut_28_choice if ("___shortcut_28_choice" in vars) else '', '___shortcut_30_choice': ___shortcut_30_choice if ("___shortcut_30_choice" in vars) else '', '___shortcut_29_choice': ___shortcut_29_choice if ("___shortcut_29_choice" in vars) else '', '___shortcut_31_choice': ___shortcut_31_choice if ("___shortcut_31_choice" in vars) else '', '___shortcut_32_choice': ___shortcut_32_choice if ("___shortcut_32_choice" in vars) else '', '___shortcut_33_choice': ___shortcut_33_choice if ("___shortcut_33_choice" in vars) else '', '___shortcut_130_choice': ___shortcut_130_choice if ("___shortcut_130_choice" in vars) else '', '___shortcut_34_choice': ___shortcut_34_choice if ("___shortcut_34_choice" in vars) else '', '___shortcut_35_choice': ___shortcut_35_choice if ("___shortcut_35_choice" in vars) else '', '___shortcut_36_choice': ___shortcut_36_choice if ("___shortcut_36_choice" in vars) else '', '___shortcut_37_choice': ___shortcut_37_choice if ("___shortcut_37_choice" in vars) else '', '___ivn_11': ___ivn_11 if ("___ivn_11" in vars) else '', '___ivn_18': ___ivn_18 if ("___ivn_18" in vars) else '', '___ivn_140': ___ivn_140 if ("___ivn_140" in vars) else '', '___shortcut_142_choice': ___shortcut_142_choice if ("___shortcut_142_choice" in vars) else '', '___shortcut_143_choice': ___shortcut_143_choice if ("___shortcut_143_choice" in vars) else '', '___shortcut_146_choice': ___shortcut_146_choice if ("___shortcut_146_choice" in vars) else '', '___shortcut_141_choice': ___shortcut_141_choice if ("___shortcut_141_choice" in vars) else '', '___shortcut_145_choice': ___shortcut_145_choice if ("___shortcut_145_choice" in vars) else '', '___shortcut_144_choice': ___shortcut_144_choice if ("___shortcut_144_choice" in vars) else '', '___ivn_15': ___ivn_15 if ("___ivn_15" in vars) else '', '___ivn_13': ___ivn_13 if ("___ivn_13" in vars) else '', '___ivn_16': ___ivn_16 if ("___ivn_16" in vars) else '', '___ivn_22': ___ivn_22 if ("___ivn_22" in vars) else '', '___shortcut_53_false': ___shortcut_53_false if ("___shortcut_53_false" in vars) else '', '___ivn_53': ___ivn_53 if ("___ivn_53" in vars) else '', '___ivn_55': ___ivn_55 if ("___ivn_55" in vars) else '', '___shortcut_56_choice': ___shortcut_56_choice if ("___shortcut_56_choice" in vars) else '', '___shortcut_57_choice': ___shortcut_57_choice if ("___shortcut_57_choice" in vars) else '', '___shortcut_58_choice': ___shortcut_58_choice if ("___shortcut_58_choice" in vars) else '', '___shortcut_59_choice': ___shortcut_59_choice if ("___shortcut_59_choice" in vars) else '', '___shortcut_24_false': ___shortcut_24_false if ("___shortcut_24_false" in vars) else '', '___ivn_24': ___ivn_24 if ("___ivn_24" in vars) else '', '___ivn_147': ___ivn_147 if ("___ivn_147" in vars) else '', '___ivn_123': ___ivn_123 if ("___ivn_123" in vars) else '', '___ivn_109': ___ivn_109 if ("___ivn_109" in vars) else '', '___ivn_49': ___ivn_49 if ("___ivn_49" in vars) else '', '___ivn_108': ___ivn_108 if ("___ivn_108" in vars) else '', '___ivn_107': ___ivn_107 if ("___ivn_107" in vars) else '', '___ivn_111': ___ivn_111 if ("___ivn_111" in vars) else '', '___ivn_128': ___ivn_128 if ("___ivn_128" in vars) else '', '___ivn_110': ___ivn_110 if ("___ivn_110" in vars) else '', '___ivn_126': ___ivn_126 if ("___ivn_126" in vars) else '', '___ivn_129': ___ivn_129 if ("___ivn_129" in vars) else '', '___ivn_127': ___ivn_127 if ("___ivn_127" in vars) else '', '___ivn_82': ___ivn_82 if ("___ivn_82" in vars) else '', '___ivn_51': ___ivn_51 if ("___ivn_51" in vars) else '', '___shortcut_96_false': ___shortcut_96_false if ("___shortcut_96_false" in vars) else '', '___ivn_96': ___ivn_96 if ("___ivn_96" in vars) else '', 'cl_session_id': cl_session_id if ("cl_session_id" in vars) else '', '___ivn_101': ___ivn_101 if ("___ivn_101" in vars) else '', '___ivn_78': ___ivn_78 if ("___ivn_78" in vars) else '', '___ivn_120': ___ivn_120 if ("___ivn_120" in vars) else '', '___ivn_115': ___ivn_115 if ("___ivn_115" in vars) else '', '___shortcut_6_false': ___shortcut_6_false if ("___shortcut_6_false" in vars) else '', '___ivn_6': ___ivn_6 if ("___ivn_6" in vars) else '', '___ivn_116': ___ivn_116 if ("___ivn_116" in vars) else '', '___ivn_114': ___ivn_114 if ("___ivn_114" in vars) else '', '___ivn_113': ___ivn_113 if ("___ivn_113" in vars) else '', '___ivn_118': ___ivn_118 if ("___ivn_118" in vars) else '', '___ivn_117': ___ivn_117 if ("___ivn_117" in vars) else '', '___ivn_122': ___ivn_122 if ("___ivn_122" in vars) else '', '___ivn_135': ___ivn_135 if ("___ivn_135" in vars) else '', '___ivn_61': ___ivn_61 if ("___ivn_61" in vars) else '', '___shortcut_63_choice': ___shortcut_63_choice if ("___shortcut_63_choice" in vars) else '', '___shortcut_65_choice': ___shortcut_65_choice if ("___shortcut_65_choice" in vars) else '', '___shortcut_64_choice': ___shortcut_64_choice if ("___shortcut_64_choice" in vars) else '', '___shortcut_66_choice': ___shortcut_66_choice if ("___shortcut_66_choice" in vars) else '', '___shortcut_62_choice': ___shortcut_62_choice if ("___shortcut_62_choice" in vars) else '', '___shortcut_2_false': ___shortcut_2_false if ("___shortcut_2_false" in vars) else '', '___ivn_2': ___ivn_2 if ("___ivn_2" in vars) else '' })
		user_defined_names = merge_two_dicts(generated_names, { "Portal_client_id": generated_names['Portal_client_id'], "Suspended_felony is false": generated_names['___shortcut_4_false'], "Suspended_felony": generated_names['___ivn_4'], "Today": generated_names['Today'], "action_taken_by_parent": generated_names['___ivn_80'], "consent_to_send_complaint_to_school is false": generated_names['___shortcut_132_false'], "consent_to_send_complaint_to_school": generated_names['___ivn_132'], "disability_suspension_time is false": generated_names['___shortcut_8_false'], "disability_suspension_time": generated_names['___ivn_8'], "filer_accommodations": generated_names['___ivn_26'], "filer_apt_number": generated_names['___ivn_14'], "filer_city": generated_names['___ivn_121'], "filer_email_address": generated_names['___ivn_17'], "filer_first_language": generated_names['___ivn_27'], "filer_first_language is \"Arabic": generated_names['___shortcut_28_choice'], "filer_first_language is \"Cape Verdean": generated_names['___shortcut_30_choice'], "filer_first_language is \"Chinese": generated_names['___shortcut_29_choice'], "filer_first_language is \"English": generated_names['___shortcut_31_choice'], "filer_first_language is \"Haitian": generated_names['___shortcut_32_choice'], "filer_first_language is \"Khmer": generated_names['___shortcut_33_choice'], "filer_first_language is \"Other Filer  Language": generated_names['___shortcut_130_choice'], "filer_first_language is \"Portuguese": generated_names['___shortcut_34_choice'], "filer_first_language is \"Russian": generated_names['___shortcut_35_choice'], "filer_first_language is \"Spanish": generated_names['___shortcut_36_choice'], "filer_first_language is \"Vietnamese": generated_names['___shortcut_37_choice'], "filer_full_name": generated_names['___ivn_11'], "filer_phone_number": generated_names['___ivn_18'], "filer_role_in_student_suspension": generated_names['___ivn_140'], "filer_role_in_student_suspension is \"Advocate": generated_names['___shortcut_142_choice'], "filer_role_in_student_suspension is \"ESE Assigned Surrogate": generated_names['___shortcut_143_choice'], "filer_role_in_student_suspension is \"Other": generated_names['___shortcut_146_choice'], "filer_role_in_student_suspension is \"Parent": generated_names['___shortcut_141_choice'], "filer_role_in_student_suspension is \"School Employee": generated_names['___shortcut_145_choice'], "filer_role_in_student_suspension is \"Student": generated_names['___shortcut_144_choice'], "filer_state_live": generated_names['___ivn_15'], "filer_street_name": generated_names['___ivn_13'], "filer_zip_code": generated_names['___ivn_16'], "first_language_not_english": generated_names['___ivn_22'], "group_of_students is false": generated_names['___shortcut_53_false'], "group_of_students": generated_names['___ivn_53'], "group_of_students_category": generated_names['___ivn_55'], "group_of_students_category is \"Classroom": generated_names['___shortcut_56_choice'], "group_of_students_category is \"Instructional": generated_names['___shortcut_57_choice'], "group_of_students_category is \"Program": generated_names['___shortcut_58_choice'], "group_of_students_category is \"School Wide": generated_names['___shortcut_59_choice'], "help_communicating is false": generated_names['___shortcut_24_false'], "help_communicating": generated_names['___ivn_24'], "other_role": generated_names['___ivn_147'], "parent_city_state_zip_student": generated_names['___ivn_123'], "parent_language_student": generated_names['___ivn_109'], "parent_name_student": generated_names['___ivn_49'], "parent_phone_student": generated_names['___ivn_108'], "parent_street_address_student": generated_names['___ivn_107'], "school__address": generated_names['___ivn_111'], "school__employee_email": generated_names['___ivn_128'], "school_district": generated_names['___ivn_110'], "school_employee_name": generated_names['___ivn_126'], "school_employee_phone": generated_names['___ivn_129'], "school_employee_title": generated_names['___ivn_127'], "school_fix_situation": generated_names['___ivn_82'], "school_name": generated_names['___ivn_51'], "send_complaint_to_dept is false": generated_names['___shortcut_96_false'], "send_complaint_to_dept": generated_names['___ivn_96'], "session_id": generated_names['cl_session_id'], "signature": generated_names['___ivn_101'], "situation_description_one": generated_names['___ivn_78'], "situation_description_two": generated_names['___ivn_120'], "student_age": generated_names['___ivn_115'], "student_disabilities is false": generated_names['___shortcut_6_false'], "student_disabilities": generated_names['___ivn_6'], "student_gender": generated_names['___ivn_116'], "student_grade": generated_names['___ivn_114'], "student_name": generated_names['___ivn_113'], "student_primary_language": generated_names['___ivn_118'], "student_street_address": generated_names['___ivn_117'], "student_town_state_zip": generated_names['___ivn_122'], "today_date": generated_names['___ivn_135'], "type_of_school_program": generated_names['___ivn_61'], "type_of_school_program is \"General Education": generated_names['___shortcut_63_choice'], "type_of_school_program is \"Home School": generated_names['___shortcut_65_choice'], "type_of_school_program is \"Home": generated_names['___shortcut_64_choice'], "type_of_school_program is \"Special Education": generated_names['___shortcut_66_choice'], "type_of_school_program is \"number_program": generated_names['___shortcut_62_choice'], "welcome_page is false": generated_names['___shortcut_2_false'], "welcome_page": generated_names['___ivn_2'] })
		word_inline_transformations = merge_two_dicts(user_defined_names, {  })
		is_defined_values = merge_two_dicts(word_inline_transformations, { '___defined_Portal_client_id': ('Portal_client_id' in vars), '___defined____shortcut_4_false': ('___shortcut_4_false' in vars), '___defined____ivn_4': ('___ivn_4' in vars), '___defined_Today': ('Today' in vars), '___defined____ivn_80': ('___ivn_80' in vars), '___defined____shortcut_132_false': ('___shortcut_132_false' in vars), '___defined____ivn_132': ('___ivn_132' in vars), '___defined____shortcut_8_false': ('___shortcut_8_false' in vars), '___defined____ivn_8': ('___ivn_8' in vars), '___defined____ivn_26': ('___ivn_26' in vars), '___defined____ivn_14': ('___ivn_14' in vars), '___defined____ivn_121': ('___ivn_121' in vars), '___defined____ivn_17': ('___ivn_17' in vars), '___defined____ivn_27': ('___ivn_27' in vars), '___defined____shortcut_28_choice': ('___shortcut_28_choice' in vars), '___defined____shortcut_30_choice': ('___shortcut_30_choice' in vars), '___defined____shortcut_29_choice': ('___shortcut_29_choice' in vars), '___defined____shortcut_31_choice': ('___shortcut_31_choice' in vars), '___defined____shortcut_32_choice': ('___shortcut_32_choice' in vars), '___defined____shortcut_33_choice': ('___shortcut_33_choice' in vars), '___defined____shortcut_130_choice': ('___shortcut_130_choice' in vars), '___defined____shortcut_34_choice': ('___shortcut_34_choice' in vars), '___defined____shortcut_35_choice': ('___shortcut_35_choice' in vars), '___defined____shortcut_36_choice': ('___shortcut_36_choice' in vars), '___defined____shortcut_37_choice': ('___shortcut_37_choice' in vars), '___defined____ivn_11': ('___ivn_11' in vars), '___defined____ivn_18': ('___ivn_18' in vars), '___defined____ivn_140': ('___ivn_140' in vars), '___defined____shortcut_142_choice': ('___shortcut_142_choice' in vars), '___defined____shortcut_143_choice': ('___shortcut_143_choice' in vars), '___defined____shortcut_146_choice': ('___shortcut_146_choice' in vars), '___defined____shortcut_141_choice': ('___shortcut_141_choice' in vars), '___defined____shortcut_145_choice': ('___shortcut_145_choice' in vars), '___defined____shortcut_144_choice': ('___shortcut_144_choice' in vars), '___defined____ivn_15': ('___ivn_15' in vars), '___defined____ivn_13': ('___ivn_13' in vars), '___defined____ivn_16': ('___ivn_16' in vars), '___defined____ivn_22': ('___ivn_22' in vars), '___defined____shortcut_53_false': ('___shortcut_53_false' in vars), '___defined____ivn_53': ('___ivn_53' in vars), '___defined____ivn_55': ('___ivn_55' in vars), '___defined____shortcut_56_choice': ('___shortcut_56_choice' in vars), '___defined____shortcut_57_choice': ('___shortcut_57_choice' in vars), '___defined____shortcut_58_choice': ('___shortcut_58_choice' in vars), '___defined____shortcut_59_choice': ('___shortcut_59_choice' in vars), '___defined____shortcut_24_false': ('___shortcut_24_false' in vars), '___defined____ivn_24': ('___ivn_24' in vars), '___defined____ivn_147': ('___ivn_147' in vars), '___defined____ivn_123': ('___ivn_123' in vars), '___defined____ivn_109': ('___ivn_109' in vars), '___defined____ivn_49': ('___ivn_49' in vars), '___defined____ivn_108': ('___ivn_108' in vars), '___defined____ivn_107': ('___ivn_107' in vars), '___defined____ivn_111': ('___ivn_111' in vars), '___defined____ivn_128': ('___ivn_128' in vars), '___defined____ivn_110': ('___ivn_110' in vars), '___defined____ivn_126': ('___ivn_126' in vars), '___defined____ivn_129': ('___ivn_129' in vars), '___defined____ivn_127': ('___ivn_127' in vars), '___defined____ivn_82': ('___ivn_82' in vars), '___defined____ivn_51': ('___ivn_51' in vars), '___defined____shortcut_96_false': ('___shortcut_96_false' in vars), '___defined____ivn_96': ('___ivn_96' in vars), '___defined_cl_session_id': ('cl_session_id' in vars), '___defined____ivn_101': ('___ivn_101' in vars), '___defined____ivn_78': ('___ivn_78' in vars), '___defined____ivn_120': ('___ivn_120' in vars), '___defined____ivn_115': ('___ivn_115' in vars), '___defined____shortcut_6_false': ('___shortcut_6_false' in vars), '___defined____ivn_6': ('___ivn_6' in vars), '___defined____ivn_116': ('___ivn_116' in vars), '___defined____ivn_114': ('___ivn_114' in vars), '___defined____ivn_113': ('___ivn_113' in vars), '___defined____ivn_118': ('___ivn_118' in vars), '___defined____ivn_117': ('___ivn_117' in vars), '___defined____ivn_122': ('___ivn_122' in vars), '___defined____ivn_135': ('___ivn_135' in vars), '___defined____ivn_61': ('___ivn_61' in vars), '___defined____shortcut_63_choice': ('___shortcut_63_choice' in vars), '___defined____shortcut_65_choice': ('___shortcut_65_choice' in vars), '___defined____shortcut_64_choice': ('___shortcut_64_choice' in vars), '___defined____shortcut_66_choice': ('___shortcut_66_choice' in vars), '___defined____shortcut_62_choice': ('___shortcut_62_choice' in vars), '___defined____shortcut_2_false': ('___shortcut_2_false' in vars), '___defined____ivn_2': ('___ivn_2' in vars) })
		return is_defined_values
---
id: 228639
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Hello! This app will help you file a Problem Resolution System (PRS) Complaint with the Massachusetts Department of Elementary & Secondary Education (DESE). A PRS Complaint asks the Massachusetts DESE to fix concerns that a person has about a student's suspension from school.


	A PRS Complaint describes:

	1. What the school did about the suspension;
	2. Whether the parent/guardian was notified about the suspension; and
	3. What concerns the person filing the complaint has about the student's suspension

	The Massachusetts DESE must look at your PRS Complaint and send a letter deciding what should happen to resolve the problem within 60 days.


	The app will ask some questions to make sure a PRS Complaint is the right form for you to complete. After that, it will ask for basic contact information from you, details about what happened with the student's suspension, and then create a PRS Complaint for you to send to the Massachusetts DESE.


	Click continue to get started.

field: ___ivn_2

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 228642
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Was the school suspension because of a felony?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_4\" data-field-id=\"371583\" ></span>": ___ivn_4
		datatype: yesnoradio
		required: true

	- html: |
			<span class="producer  collapsible-clickable" data-variable-name="___ivn_105" data-field-id="379263" ><p>If you&#39;re not sure, click here.</p>
			</span>
			<span class="producer  collapsible-hideable collapsible-area" data-variable-name="___ivn_105" data-field-id="379263" style='display: none;'><p class='collapsible-hideable' data-field-id='379263' style='display: none;'>If you were arrested for a crime, and were charged with a felony, you might have been convicted, even if you have not served time in jail.</p>
			</span>

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 228722
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Does the suspended student have any disabilities? A disability can be a learning disaibility, mental disability, or physical disability.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_6\" data-field-id=\"371587\" ></span>": ___ivn_6
		datatype: yesnoradio
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 228724
mandatory: |
	is_truthy((((augment(___ivn_6) if ("___ivn_6" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Was the student suspended for longer than 10 days?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_8\" data-field-id=\"371591\" ></span>": ___ivn_8
		datatype: yesnoradio
		required: false

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232644
mandatory: |
	is_truthy((((augment(___ivn_6) if ("___ivn_6" in vars) else Undefined()).boolean_and((augment(___ivn_8) if ("___ivn_8" in vars) else Undefined())))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Thank you for completing this form.


	Your suspension case requires a different form than the PRS Complaint because the student has disabilities and has been suspended for more than 10 days. Contact an attorney for help.


under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div><span class="is-final-block" /></div>
css: |
	<style>
		button.btn-primary[type='submit'] {
			display: none;
		}
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 228754
mandatory: |
	is_truthy((((augment(___shortcut_4_false) if ("___shortcut_4_false" in vars) else Undefined()).boolean_and((augment(___shortcut_6_false) if ("___shortcut_6_false" in vars) else Undefined())))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please provide your contact information. This will be shown on the final PRS Complaint that the application creates.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_11\" data-field-id=\"371625\" >Full name</span>": ___ivn_11
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_13\" data-field-id=\"371627\" >Street address (i.e. 123 Rainbow Road)</span>": ___ivn_13
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_14\" data-field-id=\"371628\" >Apartment or unit number</span>": ___ivn_14
		datatype: text
		required: false

	- "<span class=\"producer \" data-variable-name=\"___ivn_121\" data-field-id=\"385720\" >Town/city</span>": ___ivn_121
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_15\" data-field-id=\"371633\" >State (please type state's abbreviation, i.e. \"MA\" for Massachusetts)</span>": ___ivn_15
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_16\" data-field-id=\"371634\" >Zip code</span>": ___ivn_16
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_17\" data-field-id=\"371649\" >Email address</span>": ___ivn_17
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_18\" data-field-id=\"371650\" >Phone number</span>": ___ivn_18
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231438
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	What is your first langauge?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_27\" data-field-id=\"375504\" ></span>": ___ivn_27
		input type: dropdown
		required: true
		datatype: text
		choices:
			- "Arabic": "Arabic"
			- "Chinese": "Chinese"
			- "Cape Verdean": "Cape Verdean"
			- "English": "English"
			- "Haitian Creole": "Haitian"
			- "Khmer": "Khmer"
			- "Portuguese": "Portuguese"
			- "Russian": "Russian"
			- "Spanish": "Spanish"
			- "Vietnamese": "Vietnamese"
			- "Other": "Other Filer  Language"

	- "<span class=\"producer \" data-variable-name=\"___ivn_22\" data-field-id=\"375432\" >If your first language is not listed, please type it below.</span>": ___ivn_22
		datatype: text
		required: false

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231440
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Will you need any help speaking to the Massachusetts Department of Secondary Education? This can include an interpreter in your native language.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_24\" data-field-id=\"375436\" ></span>": ___ivn_24
		datatype: yesnoradio
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231442
mandatory: |
	is_truthy((((augment(___ivn_24) if ("___ivn_24" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	What accommodations will you need to talk with the Massachusetts Department of Secondary Education?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_26\" data-field-id=\"375503\" ></span>": ___ivn_26
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 243714
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please specify your role in the student's suspension.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_140\" data-field-id=\"391517\" ></span>": ___ivn_140
		input type: dropdown
		required: true
		datatype: text
		choices:
			- "Parent/Guardian": "Parent"
			- "Advocate": "Advocate"
			- "ESE Assigned Surrogate": "ESE Assigned Surrogate"
			- "Student": "Student"
			- "School Employee": "School Employee"
			- "Other": "Other"

	- "<span class=\"producer \" data-variable-name=\"___ivn_147\" data-field-id=\"391518\" >If you chose \"other\", please specify your role in the student's suspension</span>": ___ivn_147
		datatype: text
		required: false

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231495
mandatory: |
	is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()).boolean_not())))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Since you are not the student's parent/guardian, please provide the parent/guardian's information below.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_49\" data-field-id=\"375509\" >Parent/guardian full name</span>": ___ivn_49
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_107\" data-field-id=\"385708\" >Parent/guardian street address (i.e. 123 Rainbow Road)</span>": ___ivn_107
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_123\" data-field-id=\"385723\" >Parent/guardian city/town, state, zip code (please write their information like this: \"Boston, MA 02101\")</span>": ___ivn_123
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_108\" data-field-id=\"385709\" >Parent/guardian phone number</span>": ___ivn_108
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_109\" data-field-id=\"385710\" >Parent/guardian's primary language</span>": ___ivn_109
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231496
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please type the information about the student's school.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_51\" data-field-id=\"375510\" >School name (i.e. Boston Latin Academy)</span>": ___ivn_51
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_110\" data-field-id=\"385711\" >School District (i.e. Boston Public Schools)</span>": ___ivn_110
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_111\" data-field-id=\"385712\" >School address</span>": ___ivn_111
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231498
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Are you submitting a complaint for a group of students?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_53\" data-field-id=\"375514\" ></span>": ___ivn_53
		datatype: yesnoradio
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231499
mandatory: |
	is_truthy((((augment(___ivn_53) if ("___ivn_53" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please pick what type of group the students fall under.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_55\" data-field-id=\"375515\" ></span>": ___ivn_55
		input type: dropdown
		required: true
		datatype: text
		choices:
			- "Classroom": "Classroom"
			- "Instructional": "Instructional"
			- "Program": "Program"
			- "School Wide": "School Wide"

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 239312
mandatory: |
	is_truthy((((augment(___shortcut_53_false) if ("___shortcut_53_false" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please provide information about the student.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_113\" data-field-id=\"385713\" >Student's full name</span>": ___ivn_113
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_114\" data-field-id=\"385714\" >Student's grade (i.e. 8, 9, 10)</span>": ___ivn_114
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_115\" data-field-id=\"385715\" >Student's age</span>": ___ivn_115
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_116\" data-field-id=\"385716\" >Student's gender (male/female/nonbinary)</span>": ___ivn_116
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_117\" data-field-id=\"385717\" >Student's street address (i.e. 123 Rainbow Road)</span>": ___ivn_117
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_122\" data-field-id=\"385722\" >Student's town/city, state, and zip code (i.e. Boston, MA 02101)</span>": ___ivn_122
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_118\" data-field-id=\"385718\" >Student's primary language</span>": ___ivn_118
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 231500
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	What kind of schooling does the student get?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_61\" data-field-id=\"375516\" ></span>": ___ivn_61
		input type: dropdown
		required: true
		datatype: text
		choices:
			- 504: "number_program"
			- "General Education": "General Education"
			- "Home Hospital": "Home"
			- "Home School": "Home School"
			- "Special Education": "Special Education"

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232622
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please answer all four questions about the timing of the student's suspension in the text box below:

	1. What day was the student suspended;
	2. How long did the school say the student would be suspended for;
	3. Did the school notify a parent/guardian about the suspension; and
	4. Has a parent/guardian had the opportunity to schedule a hearing with the school about the student's suspension?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_78\" data-field-id=\"377170\" ></span>": ___ivn_78
		datatype: area
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 239313
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please answer all four questions about the circumstances of the student's suspension in the text box below:

	1. What is the reason for the student's suspension;
	2. How did the school respond after the student did whatever caused the suspension (i.e. did they bring them to the principal's office? Did the teacher speak with the student privately?);
	3. How was the student told about the suspension; and
	4. Has the school communicated with the parent/guardian since the suspension? If so, how did the school communicate with them?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_120\" data-field-id=\"385719\" ></span>": ___ivn_120
		datatype: area
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232623
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Did you do anything to try to change the school's decision to suspend the student? Please specify if you have worked with a mediator or the Bureau of Special Education Appeals (BSEA) about the student's suspension.


	If you didn't, that's OK. Click continue.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_80\" data-field-id=\"377171\" ></span>": ___ivn_80
		datatype: area
		required: false

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232624
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Is there anything that you believe the school can do to fix the situation?


	If you can't think of anything yet, that's OK. Click continue.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_82\" data-field-id=\"377172\" ></span>": ___ivn_82
		datatype: area
		required: false

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232635
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	In order to file your complaint, you need to agree to send a copy to the Massachusetts Department of Elementary & Secondary Education.


	Do you agree to send a copy of this complaint to the Massachusetts Department of Elementary & Secondary Education?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_96\" data-field-id=\"377197\" ></span>": ___ivn_96
		datatype: yesnoradio
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232643
mandatory: |
	is_truthy((((augment(___shortcut_96_false) if ("___shortcut_96_false" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	I'm sorry, but we cannot file your complaint unless you agree to let the Massachusetts Department of Elementary & Secondary Education have a copy. They need a copy of your complaint to respond.


under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div><span class="is-final-block" /></div>
css: |
	<style>
		button.btn-primary[type='submit'] {
			display: none;
		}
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 243709
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	In order to complete your complaint, you'll need to send a copy of it to the student's school. Do you consent to sending a copy of this complaint to the school?

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_132\" data-field-id=\"391509\" ></span>": ___ivn_132
		datatype: yesnoradio
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 243710
mandatory: |
	is_truthy((((augment(___shortcut_132_false) if ("___shortcut_132_false" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	I'm sorry, but we cannot complete your PRS Complaint because you did not agree to send a copy of it to the student's school.


under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div><span class="is-final-block" /></div>
css: |
	<style>
		button.btn-primary[type='submit'] {
			display: none;
		}
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 239317
mandatory: |
	is_truthy((((augment(___ivn_132) if ("___ivn_132" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please provide the contact information of a member of the school below.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_126\" data-field-id=\"385724\" >School employee's full name</span>": ___ivn_126
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_127\" data-field-id=\"385725\" >School employee's title</span>": ___ivn_127
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_128\" data-field-id=\"385726\" >School employee's email address</span>": ___ivn_128
		datatype: text
		required: true

	- "<span class=\"producer \" data-variable-name=\"___ivn_129\" data-field-id=\"385727\" >School employee's phone number</span>": ___ivn_129
		datatype: text
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 232641
mandatory: |
	is_truthy((((augment(___ivn_96) if ("___ivn_96" in vars) else Undefined()).boolean_and((augment(___ivn_132) if ("___ivn_132" in vars) else Undefined())))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please sign your name here to complete your complaint.

signature: ___ivn_101

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
id: 243711
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Please enter today's date.

fields:
	- "<span class=\"producer \" data-variable-name=\"___ivn_135\" data-field-id=\"391510\" >MM/DD/YYYY</span>": ___ivn_135
		datatype: date
		required: true

under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div></div>
css: |
	<style>
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
# synchronous lock requests for block 232642
mandatory: |
  is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()).boolean_not())))
code: |
  
---
# background assembly jobs for block 232642
event: background_assembly_task_block_232642
code: |
  assembly_results = {}
  playa_base = "https://playa.community.lawyer"
	playa_fill_endpoint = "https://playa.community.lawyer/task"
	playa_status_endpoint = "https://playa.community.lawyer/tasks"
	oxygen_base = "https://qa-oxygen.community.lawyer"
	oxygen_fill_endpoint = "https://qa-oxygen.community.lawyer/task"
	oxygen_status_endpoint = "https://qa-oxygen.community.lawyer/tasks"
	wordwizard_base = "https://wordwizard.community.lawyer"

  vars = cl_all_variables()

  def ___pdf_template_7999_to_json():
	  return {"template_data": {"url": "https://community.lawyer/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBMEt1QkE9PSIsImV4cCI6bnVsbCwicHVyIjoiYmxvYl9pZCJ9fQ==--1583107894c05824df73f0ec5c0e5897baaa486a/PRS%20Complaint%20English.pdf?disposition=attachment", "height": 1188, "width": 918, "page_count": 4}, "stamps": [{"x": 285.3333333333333, "y": 725.3333333333333, "width": 83.33333333333333, "height": 14.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_135 if ("___ivn_135" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 745.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 4, "value": string_as_bool(___ivn_132 if ("___ivn_132" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 230.0, "y": 328.66666666666663, "width": 102.66666666666666, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_22 if ("___ivn_22" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 122.66666666666666, "y": 366.0, "width": 122.66666666666666, "height": 14.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_140 if ("___ivn_140" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 314.66666666666663, "y": 350.66666666666663, "width": 226.66666666666666, "height": 11.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_147 if ("___ivn_147" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 413.3333333333333, "y": 690.6666666666666, "width": 125.33333333333333, "height": 12.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_128 if ("___ivn_128" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 414.66666666666663, "y": 708.0, "width": 118.66666666666666, "height": 13.333333333333332, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_129 if ("___ivn_129" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 104.66666666666666, "y": 726.6666666666666, "width": 144.66666666666666, "height": 20.0, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_127 if ("___ivn_127" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 278.66666666666663, "y": 746.6666666666666, "width": 145.33333333333331, "height": 14.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_126 if ("___ivn_126" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.66666666666666, "y": 705.3333333333333, "width": 214.0, "height": 15.333333333333332, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_111 if ("___ivn_111" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 73.33333333333333, "y": 202.66666666666666, "width": 465.3333333333333, "height": 94.66666666666666, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_82 if ("___ivn_82" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 334.0, "width": 464.66666666666663, "height": 97.33333333333333, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_80 if ("___ivn_80" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 530.6666666666666, "width": 464.0, "height": 142.0, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_120 if ("___ivn_120" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 73.33333333333333, "y": 606.0, "width": 463.3333333333333, "height": 74.0, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_78 if ("___ivn_78" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 449.3333333333333, "y": 120.66666666666666, "width": 96.0, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_109 if ("___ivn_109" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 448.66666666666663, "y": 231.33333333333331, "width": 94.66666666666666, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_118 if ("___ivn_118" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 72.66666666666666, "y": 104.0, "width": 262.66666666666663, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_123 if ("___ivn_123" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.0, "y": 122.66666666666666, "width": 231.33333333333331, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_107 if ("___ivn_107" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 411.3333333333333, "y": 140.66666666666666, "width": 128.66666666666666, "height": 15.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_108 if ("___ivn_108" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 201.33333333333331, "y": 140.0, "width": 125.33333333333333, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_49 if ("___ivn_49" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 71.33333333333333, "y": 213.33333333333331, "width": 265.3333333333333, "height": 14.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_122 if ("___ivn_122" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.66666666666666, "y": 230.66666666666666, "width": 235.33333333333331, "height": 13.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_117 if ("___ivn_117" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 474.66666666666663, "y": 248.0, "width": 63.33333333333333, "height": 20.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_116 if ("___ivn_116" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 342.0, "y": 250.0, "width": 22.666666666666664, "height": 18.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_115 if ("___ivn_115" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 292.66666666666663, "y": 250.0, "width": 28.666666666666664, "height": 16.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_114 if ("___ivn_114" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 105.33333333333333, "y": 250.66666666666666, "width": 151.33333333333331, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_113 if ("___ivn_113" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 385.3333333333333, "y": 310.66666666666663, "width": 156.0, "height": 26.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_26 if ("___ivn_26" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 159.33333333333331, "y": 329.3333333333333, "width": 139.33333333333331, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_27 if ("___ivn_27" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 346.0, "y": 382.66666666666663, "width": 178.0, "height": 17.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_17 if ("___ivn_17" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 143.33333333333331, "y": 382.66666666666663, "width": 164.66666666666666, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_18 if ("___ivn_18" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 105.33333333333333, "y": 398.66666666666663, "width": 53.33333333333333, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_16 if ("___ivn_16" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 472.0, "y": 419.3333333333333, "width": 32.666666666666664, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_15 if ("___ivn_15" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 313.3333333333333, "y": 417.3333333333333, "width": 124.66666666666666, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_121 if ("___ivn_121" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 137.33333333333331, "y": 418.66666666666663, "width": 119.33333333333333, "height": 12.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_13 if ("___ivn_13" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 401.3333333333333, "y": 452.0, "width": 156.0, "height": 29.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(((___ivn_101.url_for(temporary=True)) if ___ivn_101 != "" else "") if ("___ivn_101" in vars) else "")), "datatype": "signature", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 170.66666666666666, "y": 435.3333333333333, "width": 104.0, "height": 13.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_11 if ("___ivn_11" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 109.33333333333333, "y": 525.3333333333333, "width": 380.0, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_111 if ("___ivn_111" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 184.66666666666666, "y": 544.6666666666666, "width": 303.3333333333333, "height": 12.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_51 if ("___ivn_51" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 508.66666666666663, "y": 508.0, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_65_choice if ("___shortcut_65_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 421.3333333333333, "y": 507.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_62_choice if ("___shortcut_62_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 352.66666666666663, "y": 508.66666666666663, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_66_choice if ("___shortcut_66_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 248.66666666666666, "y": 507.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_63_choice if ("___shortcut_63_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 286.66666666666663, "y": 562.0, "width": 252.0, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_110 if ("___ivn_110" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}], "should_watermark": False}

	def rich_text_variable_dictionary():
	  return {
	    
	  }


	def oxygen_variables_via_word_inline_transformations(wit_whitelist):
	  retval = {}
	  
	  return retval

	# rawcontent primitives
	def rawcontent_primitives():
	  from_direct_richtext = {  }
	  from_loop_to_markdown = {  }

	  return merge_two_dicts(from_direct_richtext, from_loop_to_markdown)

	def rawcontent_value(markdown):
	  return rawcontent_primitives().get(markdown)


  if is_true(True):
	  pdf_attachment_8537_post_data = ___pdf_template_7999_to_json()
	  pdf_attachment_8537_fill_response = requests.post(playa_fill_endpoint, json=pdf_attachment_8537_post_data).json()
	  pdf_attachment_8537_task_id = pdf_attachment_8537_fill_response.get('id')
	  pdf_attachment_8537_access_key = pdf_attachment_8537_fill_response.get('key')

  

  

  





  
  

  
  if is_true(True):
	  pdf_attachment_8537_filled_attachment_url = ""
	  pdf_attachment_8537_done = False
	  pdf_attachment_8537_attempts_count = 0
	  pdf_attachment_8537_status_url = playa_status_endpoint + '/' + str(pdf_attachment_8537_task_id) + '?key=' + str(pdf_attachment_8537_access_key)
	  pa_8537_filename = as_valid_filename("PRS Complaint English")
	  fq_pa_8537_filename = pa_8537_filename + '.pdf'
	  while not pdf_attachment_8537_done and pdf_attachment_8537_attempts_count < 250:
	    pdf_attachment_8537_task_response = requests.get(pdf_attachment_8537_status_url).json()
	    pdf_attachment_8537_done = pdf_attachment_8537_task_response.get('status') == 'done'
	    pdf_attachment_8537_filled_attachment_url = playa_base + '/' + pdf_attachment_8537_task_response.get('document_url')
	    pdf_attachment_8537_attempts_count += 1
	    time.sleep(.2)

	  if pdf_attachment_8537_done:
	    block_pdf_attachment_8537 = DAFileCollection()
	    block_pdf_attachment_8537.pdf = DAFile(filename=fq_pa_8537_filename)
	    block_pdf_attachment_8537.pdf.from_url(pdf_attachment_8537_filled_attachment_url)
	    block_pdf_attachment_8537.info = {'raw': '', 'name': pa_8537_filename, 'filename': pa_8537_filename, 'description': ''}
	    assembly_results["block_pdf_attachment_8537"] = {
	      'file': block_pdf_attachment_8537,
	      'status': "success",
	    }
	  else:
	    block_pdf_attachment_8537 = None
	    assembly_results["block_pdf_attachment_8537"] = { 'status': "fail" }
	else:
	  block_pdf_attachment_8537 = None
	  assembly_results["block_pdf_attachment_8537"] = { 'status': "intentionally left conditionally unattached" }

  

  # emails
  
  # clio
  
  # cldb
  
  # google
  

  background_response_action('background_response_block_232642', assembly_results=assembly_results)
---
# background assembly response action for block 232642
event: background_response_block_232642
code: |
  # assignment
  if action_argument('assembly_results')["block_pdf_attachment_8537"]['status'] == 'success':
	  block_pdf_attachment_8537_status = "success"
	  block_pdf_attachment_8537 = action_argument('assembly_results')["block_pdf_attachment_8537"]['file']
	  block_pdf_attachment_8537_temp_url = block_pdf_attachment_8537.url_for(temporary=True, seconds=604800)
	elif action_argument('assembly_results')["block_pdf_attachment_8537"]['status'] == "intentionally left conditionally unattached":
	  block_pdf_attachment_8537_status = "intentionally left conditionally unattached"
	elif action_argument('assembly_results')["block_pdf_attachment_8537"]['status'] == "fail":
	  block_pdf_attachment_8537_status = "fail"
	  block_pdf_attachment_8537_failure_message = "A document named 'PRS Complaint English' failed to be properly prepared. Refresh the page to try preparing it again. If this issue persists, notify the app author."

  
  
  
  

  background_response()
---
# fire background jobs for block 232642
id: 232642_background_fire
mandatory: |
  is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()).boolean_not())))
code: |
  undefine('block_pdf_attachment_8537', 'block_pdf_attachment_8537_temp_url', 'block_pdf_attachment_8537_status', 'block_pdf_attachment_8537_failure_message', 'concatenated_pdf_232642', 'concatenated_pdf_232642_temp_url', 'concatenated_pdf_232642_status', 'concatenated_markdown_232642', 'concatenated_markdown_232642_temp_url', 'concatenated_markdown_232642_status', 'concatenated_markdown_attachment_232642_failure_message', 'concatenated_pdf_attachment_232642_failure_message', 'show_attachment_spinner_for_232642')
  show_attachment_spinner_for_232642 = is_true(boolean_any_true_array_reducer([True]))
  background_action('background_assembly_task_block_232642')
---
id: 232642
mandatory: |
	is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()).boolean_not())))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Congratulations! You have completed the steps necessary to file your complaint.


	Since you are not the parent of the student, you will need to obtain the parent's consent and signature on the last page of this form. This is so you can access information about the student from the Massachusetts Department of Secondary Education.


	When you obtain the student's parent's signature, mail the complaint to:

	**PRS Intake Coordinator**

	**75 Pleasant Street**

	**Malden, MA 02148-4906**


	If the student's school is a charter school and you contacted the Board of Trustees, please attach a copy of that complaint too.
		% if show_attachment_spinner_for_232642:
		<div id="cl-downloads-section">
		  <div class="sk-fading-circle">
		    <div class="sk-circle1 sk-circle"></div>
		    <div class="sk-circle2 sk-circle"></div>
		    <div class="sk-circle3 sk-circle"></div>
		    <div class="sk-circle4 sk-circle"></div>
		    <div class="sk-circle5 sk-circle"></div>
		    <div class="sk-circle6 sk-circle"></div>
		    <div class="sk-circle7 sk-circle"></div>
		    <div class="sk-circle8 sk-circle"></div>
		    <div class="sk-circle9 sk-circle"></div>
		    <div class="sk-circle10 sk-circle"></div>
		    <div class="sk-circle11 sk-circle"></div>
		    <div class="sk-circle12 sk-circle"></div>
		  </div>
		  <div style="margin-left:auto;margin-right:auto;max-width:300px;padding-top: .5em;text-align:center;line-height: 1.5em;font-size:1.1em;font-family:sans-serif;color: #5d656f;">
		    Preparing your document(s)... this may take up to a minute.
		  </div>
		</div>
		% endif


under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div><span class="is-final-block" /></div>
script: |
	<script>
		% if show_attachment_spinner_for_232642:
		var attachments = { };
		var attachmentStatus = { block_pdf_attachment_8537_status: null };
		var variablesResponse = {};
		var filenamesByUrl = {"block_pdf_attachment_8537_temp_url":"PRS Complaint English"};
		var intervalId = setInterval(function(){
		  if (Object.values(attachmentStatus).indexOf(null) < 0) {
		    window.CL.sendAnswers();
		    var attachmentKeys = Object.keys(attachments);
		    var downloadHtml = attachmentKeys.map(function(key){
		      var filename = filenamesByUrl[key];
		      var url = attachments[key];
		      return window.CL.fileDownloadButtonHtml(filename, url);
		    }).join('');
		    var failureNotices = [];
		    if (variablesResponse.block_pdf_attachment_8537_failure_message) { failureNotices.push(variablesResponse.block_pdf_attachment_8537_failure_message); }
	
				var failureHtml = failureNotices.map(function(failure){
				  return '<div class="docs">' +
				    '<div class="docs__wrapper">' +
				      '<span>' + failure + '</span>' +
				    '</div>' +
				  '</div>'
				});
	
		    $("#cl-downloads-section").html(downloadHtml + failureHtml);

		    clearInterval(intervalId);
		    return;
		  }
		  get_interview_variables(function(data) {
		    if (data.success) {
		      variablesResponse = data.variables;
		      if (data.variables.block_pdf_attachment_8537_status) { attachmentStatus["block_pdf_attachment_8537_status"] = data.variables.block_pdf_attachment_8537_status }
		      if (attachmentStatus["block_pdf_attachment_8537_status"] === 'success') { attachments["block_pdf_attachment_8537_temp_url"] = data.variables.block_pdf_attachment_8537_temp_url }
		    }
		  });
		}, 2000);
		% else:
		// no attachments
		% endif

	</script>

css: |
	<style>
		button.btn-primary[type='submit'] {
			display: none;
		}
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
# synchronous lock requests for block 239316
mandatory: |
  is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()))))
code: |
  
---
# background assembly jobs for block 239316
event: background_assembly_task_block_239316
code: |
  assembly_results = {}
  playa_base = "https://playa.community.lawyer"
	playa_fill_endpoint = "https://playa.community.lawyer/task"
	playa_status_endpoint = "https://playa.community.lawyer/tasks"
	oxygen_base = "https://qa-oxygen.community.lawyer"
	oxygen_fill_endpoint = "https://qa-oxygen.community.lawyer/task"
	oxygen_status_endpoint = "https://qa-oxygen.community.lawyer/tasks"
	wordwizard_base = "https://wordwizard.community.lawyer"

  vars = cl_all_variables()

  def ___pdf_template_7999_to_json():
	  return {"template_data": {"url": "https://community.lawyer/rails/active_storage/blobs/eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaHBBMEt1QkE9PSIsImV4cCI6bnVsbCwicHVyIjoiYmxvYl9pZCJ9fQ==--1583107894c05824df73f0ec5c0e5897baaa486a/PRS%20Complaint%20English.pdf?disposition=attachment", "height": 1188, "width": 918, "page_count": 4}, "stamps": [{"x": 285.3333333333333, "y": 725.3333333333333, "width": 83.33333333333333, "height": 14.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_135 if ("___ivn_135" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 745.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 4, "value": string_as_bool(___ivn_132 if ("___ivn_132" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 230.0, "y": 328.66666666666663, "width": 102.66666666666666, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_22 if ("___ivn_22" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 122.66666666666666, "y": 366.0, "width": 122.66666666666666, "height": 14.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_140 if ("___ivn_140" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 314.66666666666663, "y": 350.66666666666663, "width": 226.66666666666666, "height": 11.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_147 if ("___ivn_147" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 413.3333333333333, "y": 690.6666666666666, "width": 125.33333333333333, "height": 12.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_128 if ("___ivn_128" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 414.66666666666663, "y": 708.0, "width": 118.66666666666666, "height": 13.333333333333332, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_129 if ("___ivn_129" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 104.66666666666666, "y": 726.6666666666666, "width": 144.66666666666666, "height": 20.0, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_127 if ("___ivn_127" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 278.66666666666663, "y": 746.6666666666666, "width": 145.33333333333331, "height": 14.666666666666666, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_126 if ("___ivn_126" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.66666666666666, "y": 705.3333333333333, "width": 214.0, "height": 15.333333333333332, "page": 4, "value": markdown_to_plaintext(clstr(___ivn_111 if ("___ivn_111" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 73.33333333333333, "y": 202.66666666666666, "width": 465.3333333333333, "height": 94.66666666666666, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_82 if ("___ivn_82" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 334.0, "width": 464.66666666666663, "height": 97.33333333333333, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_80 if ("___ivn_80" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 74.0, "y": 530.6666666666666, "width": 464.0, "height": 142.0, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_120 if ("___ivn_120" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 73.33333333333333, "y": 606.0, "width": 463.3333333333333, "height": 74.0, "page": 2, "value": markdown_to_plaintext(clstr(___ivn_78 if ("___ivn_78" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 449.3333333333333, "y": 120.66666666666666, "width": 96.0, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_109 if ("___ivn_109" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 448.66666666666663, "y": 231.33333333333331, "width": 94.66666666666666, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_118 if ("___ivn_118" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 72.66666666666666, "y": 104.0, "width": 262.66666666666663, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_123 if ("___ivn_123" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.0, "y": 122.66666666666666, "width": 231.33333333333331, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_107 if ("___ivn_107" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 411.3333333333333, "y": 140.66666666666666, "width": 128.66666666666666, "height": 15.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_108 if ("___ivn_108" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 201.33333333333331, "y": 140.0, "width": 125.33333333333333, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_49 if ("___ivn_49" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 71.33333333333333, "y": 213.33333333333331, "width": 265.3333333333333, "height": 14.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_122 if ("___ivn_122" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 112.66666666666666, "y": 230.66666666666666, "width": 235.33333333333331, "height": 13.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_117 if ("___ivn_117" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 474.66666666666663, "y": 248.0, "width": 63.33333333333333, "height": 20.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_116 if ("___ivn_116" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 342.0, "y": 250.0, "width": 22.666666666666664, "height": 18.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_115 if ("___ivn_115" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 292.66666666666663, "y": 250.0, "width": 28.666666666666664, "height": 16.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_114 if ("___ivn_114" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 105.33333333333333, "y": 250.66666666666666, "width": 151.33333333333331, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_113 if ("___ivn_113" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 385.3333333333333, "y": 310.66666666666663, "width": 156.0, "height": 26.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_26 if ("___ivn_26" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 159.33333333333331, "y": 329.3333333333333, "width": 139.33333333333331, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_27 if ("___ivn_27" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 346.0, "y": 382.66666666666663, "width": 178.0, "height": 17.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_17 if ("___ivn_17" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 143.33333333333331, "y": 382.66666666666663, "width": 164.66666666666666, "height": 16.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_18 if ("___ivn_18" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 105.33333333333333, "y": 398.66666666666663, "width": 53.33333333333333, "height": 14.0, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_16 if ("___ivn_16" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 472.0, "y": 419.3333333333333, "width": 32.666666666666664, "height": 18.666666666666664, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_15 if ("___ivn_15" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 313.3333333333333, "y": 417.3333333333333, "width": 124.66666666666666, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_121 if ("___ivn_121" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 137.33333333333331, "y": 418.66666666666663, "width": 119.33333333333333, "height": 12.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_13 if ("___ivn_13" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 401.3333333333333, "y": 452.0, "width": 156.0, "height": 29.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(((___ivn_101.url_for(temporary=True)) if ___ivn_101 != "" else "") if ("___ivn_101" in vars) else "")), "datatype": "signature", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 170.66666666666666, "y": 435.3333333333333, "width": 104.0, "height": 13.333333333333332, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_11 if ("___ivn_11" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 109.33333333333333, "y": 525.3333333333333, "width": 380.0, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_111 if ("___ivn_111" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 184.66666666666666, "y": 544.6666666666666, "width": 303.3333333333333, "height": 12.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_51 if ("___ivn_51" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 508.66666666666663, "y": 508.0, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_65_choice if ("___shortcut_65_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 421.3333333333333, "y": 507.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_62_choice if ("___shortcut_62_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 352.66666666666663, "y": 508.66666666666663, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_66_choice if ("___shortcut_66_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 248.66666666666666, "y": 507.3333333333333, "width": 10.666666666666666, "height": 10.666666666666666, "page": 1, "value": string_as_bool(___shortcut_63_choice if ("___shortcut_63_choice" in vars) else ""), "datatype": "checkbox", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}, {"x": 286.66666666666663, "y": 562.0, "width": 252.0, "height": 10.666666666666666, "page": 1, "value": markdown_to_plaintext(clstr(___ivn_110 if ("___ivn_110" in vars) else "")), "datatype": "text", "context_height": 792.0, "context_width": 612.0, "font_size": 12, "font_color": "000000"}], "should_watermark": False}

	def rich_text_variable_dictionary():
	  return {
	    
	  }


	def oxygen_variables_via_word_inline_transformations(wit_whitelist):
	  retval = {}
	  
	  return retval

	# rawcontent primitives
	def rawcontent_primitives():
	  from_direct_richtext = {  }
	  from_loop_to_markdown = {  }

	  return merge_two_dicts(from_direct_richtext, from_loop_to_markdown)

	def rawcontent_value(markdown):
	  return rawcontent_primitives().get(markdown)


  if is_true(True):
	  pdf_attachment_8538_post_data = ___pdf_template_7999_to_json()
	  pdf_attachment_8538_fill_response = requests.post(playa_fill_endpoint, json=pdf_attachment_8538_post_data).json()
	  pdf_attachment_8538_task_id = pdf_attachment_8538_fill_response.get('id')
	  pdf_attachment_8538_access_key = pdf_attachment_8538_fill_response.get('key')

  

  

  





  
  

  
  if is_true(True):
	  pdf_attachment_8538_filled_attachment_url = ""
	  pdf_attachment_8538_done = False
	  pdf_attachment_8538_attempts_count = 0
	  pdf_attachment_8538_status_url = playa_status_endpoint + '/' + str(pdf_attachment_8538_task_id) + '?key=' + str(pdf_attachment_8538_access_key)
	  pa_8538_filename = as_valid_filename("PRS Complaint English")
	  fq_pa_8538_filename = pa_8538_filename + '.pdf'
	  while not pdf_attachment_8538_done and pdf_attachment_8538_attempts_count < 250:
	    pdf_attachment_8538_task_response = requests.get(pdf_attachment_8538_status_url).json()
	    pdf_attachment_8538_done = pdf_attachment_8538_task_response.get('status') == 'done'
	    pdf_attachment_8538_filled_attachment_url = playa_base + '/' + pdf_attachment_8538_task_response.get('document_url')
	    pdf_attachment_8538_attempts_count += 1
	    time.sleep(.2)

	  if pdf_attachment_8538_done:
	    block_pdf_attachment_8538 = DAFileCollection()
	    block_pdf_attachment_8538.pdf = DAFile(filename=fq_pa_8538_filename)
	    block_pdf_attachment_8538.pdf.from_url(pdf_attachment_8538_filled_attachment_url)
	    block_pdf_attachment_8538.info = {'raw': '', 'name': pa_8538_filename, 'filename': pa_8538_filename, 'description': ''}
	    assembly_results["block_pdf_attachment_8538"] = {
	      'file': block_pdf_attachment_8538,
	      'status': "success",
	    }
	  else:
	    block_pdf_attachment_8538 = None
	    assembly_results["block_pdf_attachment_8538"] = { 'status': "fail" }
	else:
	  block_pdf_attachment_8538 = None
	  assembly_results["block_pdf_attachment_8538"] = { 'status': "intentionally left conditionally unattached" }

  

  # emails
  
  # clio
  
  # cldb
  
  # google
  

  background_response_action('background_response_block_239316', assembly_results=assembly_results)
---
# background assembly response action for block 239316
event: background_response_block_239316
code: |
  # assignment
  if action_argument('assembly_results')["block_pdf_attachment_8538"]['status'] == 'success':
	  block_pdf_attachment_8538_status = "success"
	  block_pdf_attachment_8538 = action_argument('assembly_results')["block_pdf_attachment_8538"]['file']
	  block_pdf_attachment_8538_temp_url = block_pdf_attachment_8538.url_for(temporary=True, seconds=604800)
	elif action_argument('assembly_results')["block_pdf_attachment_8538"]['status'] == "intentionally left conditionally unattached":
	  block_pdf_attachment_8538_status = "intentionally left conditionally unattached"
	elif action_argument('assembly_results')["block_pdf_attachment_8538"]['status'] == "fail":
	  block_pdf_attachment_8538_status = "fail"
	  block_pdf_attachment_8538_failure_message = "A document named 'PRS Complaint English' failed to be properly prepared. Refresh the page to try preparing it again. If this issue persists, notify the app author."

  
  
  
  

  background_response()
---
# fire background jobs for block 239316
id: 239316_background_fire
mandatory: |
  is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()))))
code: |
  undefine('block_pdf_attachment_8538', 'block_pdf_attachment_8538_temp_url', 'block_pdf_attachment_8538_status', 'block_pdf_attachment_8538_failure_message', 'concatenated_pdf_239316', 'concatenated_pdf_239316_temp_url', 'concatenated_pdf_239316_status', 'concatenated_markdown_239316', 'concatenated_markdown_239316_temp_url', 'concatenated_markdown_239316_status', 'concatenated_markdown_attachment_239316_failure_message', 'concatenated_pdf_attachment_239316_failure_message', 'show_attachment_spinner_for_239316')
  show_attachment_spinner_for_239316 = is_true(boolean_any_true_array_reducer([True]))
  background_action('background_assembly_task_block_239316')
---
id: 239316
mandatory: |
	is_truthy((((augment(___shortcut_141_choice) if ("___shortcut_141_choice" in vars) else Undefined()))))
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Congratulations! You have completed the steps necessary to file your complaint.


	Since you are the parent of the student, you can ignore the portion on the last page of this form asking for third party consent.


	Send the completed form to: Compliance@doe.mass.edu, and make the subject "LAST NAME PRS Intake Form"


	If the student's school is a charter school and you contacted the Board of Trustees, please attach a copy of that complaint in the email too.
		% if show_attachment_spinner_for_239316:
		<div id="cl-downloads-section">
		  <div class="sk-fading-circle">
		    <div class="sk-circle1 sk-circle"></div>
		    <div class="sk-circle2 sk-circle"></div>
		    <div class="sk-circle3 sk-circle"></div>
		    <div class="sk-circle4 sk-circle"></div>
		    <div class="sk-circle5 sk-circle"></div>
		    <div class="sk-circle6 sk-circle"></div>
		    <div class="sk-circle7 sk-circle"></div>
		    <div class="sk-circle8 sk-circle"></div>
		    <div class="sk-circle9 sk-circle"></div>
		    <div class="sk-circle10 sk-circle"></div>
		    <div class="sk-circle11 sk-circle"></div>
		    <div class="sk-circle12 sk-circle"></div>
		  </div>
		  <div style="margin-left:auto;margin-right:auto;max-width:300px;padding-top: .5em;text-align:center;line-height: 1.5em;font-size:1.1em;font-family:sans-serif;color: #5d656f;">
		    Preparing your document(s)... this may take up to a minute.
		  </div>
		</div>
		% endif


under: |
	<hr>	<div style='display:flex;justify-content:space-between;'><div>Powered by [Community.lawyer](https://community.lawyer/)</div><div class='sac-wrapper'>[Save and continue later](${'https://community.lawyer/app' + interview_url(local=True, interview_id=9723).replace('/interview?', '?')})</div><span class="is-final-block" /></div>
script: |
	<script>
		% if show_attachment_spinner_for_239316:
		var attachments = { };
		var attachmentStatus = { block_pdf_attachment_8538_status: null };
		var variablesResponse = {};
		var filenamesByUrl = {"block_pdf_attachment_8538_temp_url":"PRS Complaint English"};
		var intervalId = setInterval(function(){
		  if (Object.values(attachmentStatus).indexOf(null) < 0) {
		    window.CL.sendAnswers();
		    var attachmentKeys = Object.keys(attachments);
		    var downloadHtml = attachmentKeys.map(function(key){
		      var filename = filenamesByUrl[key];
		      var url = attachments[key];
		      return window.CL.fileDownloadButtonHtml(filename, url);
		    }).join('');
		    var failureNotices = [];
		    if (variablesResponse.block_pdf_attachment_8538_failure_message) { failureNotices.push(variablesResponse.block_pdf_attachment_8538_failure_message); }
	
				var failureHtml = failureNotices.map(function(failure){
				  return '<div class="docs">' +
				    '<div class="docs__wrapper">' +
				      '<span>' + failure + '</span>' +
				    '</div>' +
				  '</div>'
				});
	
		    $("#cl-downloads-section").html(downloadHtml + failureHtml);

		    clearInterval(intervalId);
		    return;
		  }
		  get_interview_variables(function(data) {
		    if (data.success) {
		      variablesResponse = data.variables;
		      if (data.variables.block_pdf_attachment_8538_status) { attachmentStatus["block_pdf_attachment_8538_status"] = data.variables.block_pdf_attachment_8538_status }
		      if (attachmentStatus["block_pdf_attachment_8538_status"] === 'success') { attachments["block_pdf_attachment_8538_temp_url"] = data.variables.block_pdf_attachment_8538_temp_url }
		    }
		  });
		}, 2000);
		% else:
		// no attachments
		% endif

	</script>

css: |
	<style>
		button.btn-primary[type='submit'] {
			display: none;
		}
		:root {
		  --body-color: #F9F9F9;
		  --navbar-color: #f8f9fa;
		  --text-color: #212529;
		  --primary-btn-background-color: #007bff;
		  --primary-btn-background-color-hover: #0062cc;
		  --primary-btn-text-color: #fff;
		  --secondary-btn-background-color: #FFFFFF;
		  --secondary-btn-background-color-hover: #e6e6e6;
		  --secondary-btn-text-color: #212529;
		  --text-link-color: #007bff;
		  --progress-bar-color: #007bff;
		  --hide-navbar-background-color: #fff;
		}

	</style>
---
mandatory: |
	True
question: |
	<span id="session-id" data-session-id="${user_info().session}">
	<span id="sends-answers" data-sends-answers="true">
	<span id="cl-endpoint" data-cl-endpoint="https://community.lawyer">
	<span id="interview-id" data-interview-id="9723">

subquestion: |
	Thank you for using this app. Your session is complete.
---
