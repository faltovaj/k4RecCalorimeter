################################################################################
# Package: RecFCCeeCalorimeter
################################################################################

file(GLOB _module_sources src/components/*.cpp)
gaudi_add_module(k4RecFCCeeCalorimeterPlugins
                 SOURCES ${_module_sources}
                 LINK k4FWCore::k4FWCore
                      k4FWCore::k4Interface
                      Gaudi::GaudiAlgLib
                      Gaudi::GaudiKernel
                      DD4hep::DDCore
                      EDM4HEP::edm4hep
                      FCCDetectors::DetSegmentation
                      DD4hep::DDG4
                      ROOT::Core
                      ROOT::Hist
                      )

install(TARGETS k4RecFCCeeCalorimeterPlugins
  EXPORT k4RecCalorimeterTargets
  RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}" COMPONENT bin
  LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}" COMPONENT shlib
  COMPONENT dev)


install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/tests DESTINATION ${CMAKE_INSTALL_DATADIR}/${CMAKE_PROJECT_NAME}/RecFCCeeCalorimeter)



add_test(NAME FCCeeLAr_reproduceSegfault
         COMMAND k4run RecFCCeeCalorimeter/tests/options/reproduceSegfault.py
         WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
add_test(NAME FCCeeLAr_simulateForReco
         COMMAND k4run RecFCCeeCalorimeter/tests/options/runCaloSim.py
         WORKING_DIRECTORY ${PROJECT_SOURCE_DIR})
add_test(NAME FCCeeLAr_slidingWindowClustering
         COMMAND k4run RecFCCeeCalorimeter/tests/options/runFullCaloSystem_ReconstructionSW_noiseFromFile.py
         WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}
           )
set_tests_properties(FCCeeLAr_slidingWindowClustering PROPERTIES DEPENDS "FCCeeLAr_simulateForReco")
