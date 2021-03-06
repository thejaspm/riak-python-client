RIAK_CONF = ${RIAK_DIR}/etc/riak.conf
# ADVANCED_CONF = ${RIAK_DIR}/etc/advanced.config
# RIAK = ${RIAK_DIR}/bin/riak
RIAK_ADMIN = ${RIAK_DIR}/bin/riak-admin
CERTS_DIR = $(shell pwd)/../riak/tests/resources

preconfigure:
	@../setup.py preconfigure --riak-conf=${RIAK_CONF}

configure:
	@../setup.py configure --riak-admin=${RIAK_ADMIN}

compile:
	@../setup.py develop

lint:
	@pip install --upgrade pep8 pyflakes
	@cd ..; pep8 riak *.py
	@cd ..; pyflakes riak *.py
	@openssl verify -CAfile ${CERTS_DIR}/ca.crt ${CERTS_DIR}/client.crt
	@openssl verify -CAfile ${CERTS_DIR}/ca.crt ${CERTS_DIR}/server.crt

test: setup test_normal test_security

test_normal:
	@echo "Testing Riak Python Client (without security)"
	@../setup.py disable_security --riak-admin=${RIAK_ADMIN}
	@RUN_YZ=1 SKIP_DATATYPES=0 SKIP_INDEXES=0 ./tox_runner.sh ..

test_security:
	@echo "Testing Riak Python Client (with security)"
	@../setup.py enable_security --riak-admin=${RIAK_ADMIN}
	@RUN_YZ=1 SKIP_INDEXES=0 RUN_SECURITY=1 SKIP_POOL=1 SKIP_RESOLVE=1 RIAK_TEST_HTTP_PORT=18098 ./tox_runner.sh ..

# These are required to actually build all the Python versions:
# * pip install tox
# * pyenv - https://github.com/yyuu/pyenv
# And two pyenv plugins:
# * pyenv virtualenv - https://github.com/yyuu/pyenv-virtualenv
# * pyenv alias - https://github.com/s1341/pyenv-alias
setup:
	./tox_setup.sh
