Logic
=====

Deploy
------

::

    @xlearn_view.route('/algorithm/add', methods=['POST'])
    def algorithm_add():
        input_data = request.form
        logger.info('Data: {}'.format(input_data))
        param_priority = {
            'name': PriorityInfo.NECESSARY,
            'display_name': PriorityInfo.OPTIONAL,
            'description': PriorityInfo.OPTIONAL,
            'entry_name': PriorityInfo.NECESSARY,
            'algorithm_type': PriorityInfo.OPTIONAL,
            'hyperparameter_config': PriorityInfo.OPTIONAL,
            'available': PriorityInfo.OPTIONAL,
            'train_input_pattern': PriorityInfo.OPTIONAL,
            'train_output_pattern': PriorityInfo.OPTIONAL,
            'model_input_pattern': PriorityInfo.OPTIONAL,
            'model_output_pattern': PriorityInfo.OPTIONAL,
            'runtime': PriorityInfo.OPTIONAL,
            'learning': PriorityInfo.OPTIONAL,
            'package_id': PriorityInfo.OPTIONAL
        }
        try:
            metadata = check_params(input_data, param_priority)
            params = dict()
            params['metadata'] = metadata
            # TODO exception name
            params['filename'] = input_data.get('filename')
            params['action'] = 'start'
            algorithm_id = algorithm_process.execute(params=params)
        except XlearnException as e:
            return JSONWrapper.fail(exception_info=e.MSG, status=e.STATUS)
        except Exception as e:
            logger.error(e)
            logger.error(traceback.format_exc())
            return JSONWrapper.fail(exception_info=UNDEFINE.MSG)

        return JSONWrapper.success(algorithm_id)

Publish
-------

Install $project by running:

    install project

Train
-----

