===================
pb.jsnapy.ospf.yaml
===================

-----------------------
validate OSPF neighbors
-----------------------

jsnapy.ospf
===========

This module will validate the OSPF neighbors on a JunOS device

Example
-------

Here is a basic example:

.. code-block:: yaml

    ### ---------------------------------------------------------------------------
    ### JSNAPY TESTS
    ### ---------------------------------------------------------------------------
    - hosts: all
      connection: local
      gather_facts: False
      become: False
      roles: 
        - juniper.junos
      tasks:

        # #################################################
        # ### Check OSPF Neighbors                       ###
        # #################################################
        - name: "### CHECK OSPF NEIGHBORS"
          juniper_junos_jsnapy:
            # define remote device parameters
            host: "{{ ansible_host }}"
            user: "automation"
            passwd: "juniper123"
  
            # define our test file and action
            test_files: tests/ospf.yaml
            action: snapcheck

          # store the output of our test in a new object test_ospf
          register: test_ospf

          # disable failed tests from cancelling the rest of our play
          ignore_errors: True

          # tag the result as test_ospf
          tags: [ test_ospf ]

        - name: "print test_ospf object to screen"
          debug:
            msg: "{{ test_ospf }}"
          # when:
          #   - "test_ospf.passPercentage != 100"

        - name: Check Alarms Test
          assert:
            that:
              - "test_ospf.passPercentage == 100"
          tags: [ test_ospf ]
